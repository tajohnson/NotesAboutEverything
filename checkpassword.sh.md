#!/bin/bash

# =================================================
# python require simple-pbkdf2 and passlib package!
# =================================================

#DB="mr_master_small"
#DB_USER="root"
#DB_PASS=""
#DB_HOST="127.0.0.1"
DB="mroute_admin"
DB_USER="mroute_admin"
DB_PASS="suddenlyarrangeexclaimednearest"
DB_HOST="199.89.0.45"



# The first and only argument is path to checkpassword-reply binary.
# It should be executed at the end if authentication succeeds.
CHECKPASSWORD_REPLY_BINARY="$1"

# Messages to stderr will end up in mail log (prefixed with "dovecot: auth: Error:")
LOG=/dev/stderr

# User and password will be supplied on file descriptor 3.
INPUT_FD=3

# Error return codes.
ERR_PERMFAIL=1
ERR_NOUSER=3
ERR_TEMPFAIL=111

# Make testing this script easy. To check it just run:
#   printf '%s\x0%s\x0' <user> <password> | ./checkpassword.sh test; echo "$?"
if [ "$CHECKPASSWORD_REPLY_BINARY" = "test" ]; then
    CHECKPASSWORD_REPLY_BINARY=/bin/true
    INPUT_FD=0
fi


# Just a simple logging helper.
log_result()
{
    echo "$*; Input: $USER; Reply binary: $CHECKPASSWORD_REPLY_BINARY" 1>&2
}


password_check()
{
    local localpart="$1"
    local domain="$2"
    local password="$3"

    #local hashed_pass=`mysql -u $DB_USER -p$DB_PASS -h $DB_HOST -e "SELECT password FROM auth_user WHERE username='$username'" --skip-column-names $DB`
    local hashed_pass=`mysql -u $DB_USER -p$DB_PASS -h $DB_HOST -e "select password from auth_user u left join amavisd_emailaccount e on u.id=e.user_id left join amavisd_localpartalias la on e.id=la.email_account_id left join amavisd_domainalias d on la.domain_id=d.domain_id left join temporary_hosted h on d.domain_id=h.domain_id where la.localpart='$localpart' and d.name = '$domain' and (h.hosted=1 or h.continuity=1) LIMIT 1" --skip-column-names $DB`

    if [ -n "$hashed_pass" ]; then
        local split=(${hashed_pass//$/ })
        if [ ${#split[@]} != 5 ] && [ ${#split[@]} != 4 ]; then
            echo 0
            return
        fi

        local alg
        local iterations
        local salt
        local hash

        local generated_hash

        if [ ${#split[@]} == 5 ]; then
            alg=${split[0]}
            local iter=${split[2]}
            local iterations_split=(${iter//=/ })
            iterations=${iterations_split[1]}
            salt=${split[3]}
            hash=${split[4]}

            generated_hash=`python -c "from base64 import b64encode; from passlib.hash import sha256_crypt; import hashlib; \
                print sha256_crypt.encrypt('$password', salt='$salt', rounds=$iterations).split('$')[-1]"`
                #print sha256_crypt.hash('$password', salt='$salt', rounds=$iterations).split('$')[-1]"`

        else
            alg=${split[0]}
            iterations=${split[1]}
            salt=${split[2]}
            hash=${split[3]}

            generated_hash=`python -c "from base64 import b64encode; from pbkdf2 import pbkdf2_bin; import hashlib; \
                print b64encode(pbkdf2_bin('$password', '$salt', $iterations, 32, hashlib.sha256)).decode('ascii').strip()"`
        fi

#        log_result "$alg $iterations $salt $hash"

#        log_result $generated_hash

        if [ "$generated_hash" == "$hash" ]; then
            echo 1
            return
        else
            echo 0
            return
        fi
    fi

    return 0
}


password_lookup()
{
    local localpart="$1"
    local domain="$2"

    log_result $1 $2

    # select * from auth_user u
    #    left join amavisd_emailaccount e on u.id=e.user_id
    #    left join amavisd_localpartalias la on e.id=la.email_account_id
    #    left join amavisd_domainalias d on la.domain_id=d.domain_id
    #    where la.localpart='23' and d.name = 'asdf2.com'

    #hashed_pass=`mysql -u $DB_USER -p$DB_PASS -h $DB_HOST -e "SELECT password FROM auth_user WHERE username='$username'" --skip-column-names $DB`
    hashed_pass=`mysql -u $DB_USER -p$DB_PASS -h $DB_HOST -e "select password from auth_user u left join amavisd_emailaccount e on u.id=e.user_id left join amavisd_localpartalias la on e.id=la.email_account_id left join amavisd_domainalias d on la.domain_id=d.domain_id left join temporary_hosted h on d.domain_id=h.domain_id where la.localpart='$localpart' and d.name = '$domain' and (h.hosted=1 or h.continuity=1) limit 1" --skip-column-names $DB`
    if [ -n "$hashed_pass" ]; then
        echo 1
        return
    else
        echo 0
        return
    fi
}

# Read input data. It is available from $INPUT_FD as "${USER}\0${PASS}\0".
# Password may be empty if not available (i.e. if doing credentials lookup).
read -d $'\0' -r -u $INPUT_FD USER
read -d $'\0' -r -u $INPUT_FD PASS

# Both mailbox and domain directories should be in lowercase on file system.
# So let's convert login user name to lowercase and tell Dovecot that 'user' and 'home'
# (which overrides 'mail_home' global parameter) values should be updated.
# Of course, conversion to lowercase may be done in Dovecot configuration as well.
export USER="`echo \"$USER\" | tr 'A-Z' 'a-z'`"

LOCALPART="`echo \"$USER\" | awk -F '@' '{ print $1 }'`"
DOMAIN="`echo \"$USER\" | awk -F '@' '{ print $2 }'`"


# CREDENTIALS_LOOKUP=1 environment is set when doing non-plaintext authentication.
if [ "$CREDENTIALS_LOOKUP" = 1 ]; then
    action=password_lookup
else
    action=password_check
fi

# Perform credentials lookup/verification.
lookup_result=`$action "$LOCALPART" "$DOMAIN" "$PASS"` || {
    # If it failed, consider it an internal temporary error.
    # This usually happens due to permission problems.
    log_result "internal error (ran as `id`)"
    exit $ERR_TEMPFAIL
}

if [ "$lookup_result" == 1 ]; then
    # Dovecot calls the script with AUTHORIZED=1 environment set when performing a userdb lookup.
    # The script must acknowledge this by changing the environment to AUTHORIZED=2,
    # otherwise the lookup fails.
    [ "$AUTHORIZED" != 1 ] || export AUTHORIZED=2

    # And here's how to return extra fields from userdb/passdb lookup, e.g. 'uid' and 'gid'.
    # See also:
    #   http://wiki2.dovecot.org/PasswordDatabase/ExtraFields
    #   http://wiki2.dovecot.org/UserDatabase/ExtraFields
    #   http://wiki2.dovecot.org/VirtualUsers
    #export userdb_uid=mailowner #todo: change to actual user
    #export userdb_gid=mailowner
    #export userdb_mail=maildir:/data/mailstorage/${DOMAIN:0:1}/${DOMAIN:1:1}/${DOMAIN}/${LOCALPART:0:1}/${LOCALPART}/mail

    export EXTRA="userdb_uid userdb_gid userdb_mail $EXTRA"
    #export EXTRA="userdb_uid userdb_gid $EXTRA"

#    log_result "lookup result: '$lookup_result'"

    # At the end of successful authentication execute checkpassword-reply binary.
    exec $CHECKPASSWORD_REPLY_BINARY
else
    # If matching credentials were not found, return proper error code depending on lookup mode.
    if [ "$AUTHORIZED" = 1 -a "$CREDENTIALS_LOOKUP" = 1 ]; then
        log_result "lookup failed (user not found)"
        exit $ERR_NOUSER
    else
        log_result "lookup failed (credentials are invalid)"
        exit $ERR_PERMFAIL
    fi
fi
