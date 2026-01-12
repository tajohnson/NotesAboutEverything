backend: /var/www/ui2-backend
the ui2 main branch from the same production repo (git@github.com:tajohnson/controlpanel.git) 
git checkout ui2/main


frontend /var/www/ui2-frontend-src
use the cp2 repo https://github.com/tajohnson/cp2
master branch


cd /var/www/ui2-frontend-src
git pull
yarn build
rsync -avzr -e ssh ./dist/ ../ui2-frontend-dist/