virtualenv --python=/usr/bin/python27 /home/mroute/venv/sentry
source /home/mroute/venv/sentry/bin/activate
pip install sentry



mysql
user: sentry




# set up new admin_dev environment:

virtualenv --python=/usr/bin/python27 /home/mroute/venv/admin_dev
source /home/mroute/venv/admin_dev/bin/activate; cd /home/mroute/webapp/admin_dev

pip install --upgrade pip
pip install -r requirements.txt --no-binary greenlet

pip install nodeenv
nodeenv -p
npm install -g bower

bower --allow-root install bower.json   # yes, I know, but I'm root on this machine

This warning occurs:


Please note that,
    mailroute depends on angular#1.4.5 which resolved to angular#1.4.5
    angular-ui-validate#1.1.1 depends on angular#>= 1.0.5 which resolved to angular#1.4.5
    angular-ui-date#0.0.11 depends on angular#~1.x which resolved to angular#1.4.5
    angular-resource#1.4.14, angular-route#1.4.14, angular-sanitize#1.4.14 depends on angular#1.4.14 which resolved to angular#1.4.14
Resort to using angular#1.4.5 which resolved to angular#1.4.5
Code incompatibilities may occur.





