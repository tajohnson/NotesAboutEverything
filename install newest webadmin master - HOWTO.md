
on 199.89.0.45

mysqldump -u root -p mroute_admin > /tmp/backup.sql





HOWTO admin_dev  vue branch

scl enable python27 bash

virtualenv /home/mroute/venv/admin_dev
vi /home/mroute/venv/admin_dev/bin/activate
	add to line 39
		source /opt/rh/python27/enable


source /home/mroute/venv/admin_dev/bin/activate;  
pip install --upgrade pip
cd /home/mroute/webapp/admin_dev  

git fetch
git checkout master
git pull




pip install --upgrade -r ./requirements.txt 

pip install nodeenv
nodeenv -p --node=8.16.1 --npm=6.4.1

source /home/mroute/venv/admin_dev/bin/activate;
node --version




rm -rf ./webadmin/vue/node_modules
rm -rf ./webadmin/static/js/vue/node_modules


cd /home/mroute/webapp/admin_dev/webadmin/vue
npm install
npm run build

cd /home/mroute/webapp/admin_dev
./manage.py collectstatic

./manage.py migrate



--------for production


HOWTO admin_dev  vue branch

# make a backup copy of admin database
mysqldump -u root -p mroute_admin > /tmp/backup.sql
create database admin_backup;
use admin_backup;
source /tmp/backup.sql


scl enable python27 bash

virtualenv /home/mroute/venv/admin
vi /home/mroute/venv/admin/bin/activate
	add to line 39
		source /opt/rh/python27/enable


source /home/mroute/venv/admin/bin/activate;  
pip install --upgrade pip
cd /home/mroute/webapp/admin  

git fetch
git checkout master
git pull




pip install  -r ./requirements.txt 

pip install nodeenv
nodeenv -p --node=6.11.3 --npm=3.10.10
source /home/mroute/venv/admin/bin/activate;
node --version



cp /tmp/admin-settings/*.py /home/mroute/webapp/admin/webadmin/conf/settings/


rm -rf /home/mroute/venv/admin/webadmin/vue/node_modules
rm -rf /home/mroute/venv/admin/webadmin/static/js/vue/node_modules


cd /home/mroute/webapp/admin/webadmin/vue
npm install
npm run build

npm -g install uglify-js
npm -g install yuglify


cd /home/mroute/webapp/admin

./manage.py collectstatic


./manage.py migrate

/etc/init.d/gunicorn-admin restart




update to add yubikey to production 

scl enable python27 bash

virtualenv /home/mroute/venv/admin_dev2
vi /home/mroute/venv/admin_dev2/bin/activate
	add to line 39
		source /opt/rh/python27/enable


source /home/mroute/venv/admin_dev2/bin/activate;  
pip install --upgrade pip
cd /home/mroute/webapp/admin_dev2  
pip install  -r ./requirements.txt 

pip install nodeenv
nodeenv -p --node=8.16.1  --npm=6.4.1
source /home/mroute/venv/admin_dev2/bin/activate;
node --version
rm -rf /home/mroute/venv/admin_dev2/webadmin/vue/node_modules
rm -rf /home/mroute/venv/admin_dev2/webadmin/static/js/vue/node_modules


cd /home/mroute/webapp/admin_dev2/webadmin/vue
npm install
npm run build

npm -g install uglify-js
npm -g install yuglify


cd /home/mroute/webapp/admin_dev2


./manage.py collectstatic


./manage.py migrate

