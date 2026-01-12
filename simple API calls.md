Add 1 or more users to a domain (defaults to system-generated password that the user can change themselves through the "forgot my password" link):

curl --dump-header - -X POST  \
-H 'Content-Type: application/json' \
-H 'Authorization: ApiKey <yourlogin>:<yourapikey>' \ 
--data ' {
    "localparts" : ["user1", "user2"], 
    "domain": "/api/v1/domain/domain.com/"
    }' \
https://admin.mailroute.net/api/v1/email_account/mass_add/


Delete a user:

curl --dump-header - -X DELETE \
-H 'Content-Type: application/json' \
-H 'Authorization: ApiKey <yourlogin>:<yourapikey>' \ 
https://admin.mailroute.net/api/v1/email_account/user@domain.com/


Adding an alias to an existing account:

curl --dump-header - -X POST \
-H 'Content-Type: application/json' \
-H 'Authorization: ApiKey <yourlogin>:<yourapikey>' \ 
--data '  {
    "email_account" : "/api/v1/email_account/user@domain.com/", 
    "localpart" : "useralias"
  }' \
https://admin.mailroute.net/api/v1/localpart_alias/

Delete an alias:

curl --dump-header - -X DELETE \
-H 'Content-Type: application/json' \
-H 'Authorization: ApiKey <yourlogin>:<yourapikey>' \ 
https://admin.mailroute.net/api/v1/localpart_alias/a999@domain.com/


Change Password:

curl -s --dump-header - -X PATCH -H 'Content-Type: application/json' -H 'Authorization: ApiKey u1@usercase.com:<key>' --data '{
    "confirm_password": "qazxswedc",
    "password": "qazxswedc",
    "change_pwd": "1"
}' 'https://admin.mailroute.net/api/v1/email_account/user3@apitest.com/'
