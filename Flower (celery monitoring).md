

Runs on http://0.0.0.0:5555 on each celery server.

Can connect to it by setting up a tunnel:

```
ssh -L 15555:localhost:5555 -J root@199.89.1.103  root@002.lax.mailroute.net
ssh -L 25555:localhost:5555 -J root@199.89.1.103  root@005.lax.mailroute.net
ssh -L 35555:localhost:5555 -J root@199.89.1.103  root@001.mia.mailroute.net

```

# starting flower by hand (now have services installed)

```
 #  source /var/venv/production/bin/activate
 #  cd /var/www/production
 #  celery -A webadmin flower
```