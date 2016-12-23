# ansible-nextcloud
Ansible role to install a docker containerized Nextcloud instance

Components :
- [wonderfall/nextcloud docker image](https://hub.docker.com/r/wonderfall/nextcloud/)
- [Official mariadb docker image](https://hub.docker.com/_/mariadb/)
- [Official nginx docker image](https://hub.docker.com/_/nginx/)
- [Minimal Let's Encrypt webroot docker image](https://github.com/RLejolivet/docker-letsencrypt-webroot)

Assumes the target machine does not have any web server.

Any firewall should open ports 80 (for Let's Encrypt verification process) and 443 (HTTPS access to the Nextcloud instance)

Current installation steps:

1. ```apt-get install python python-docker docker.io```
2. Run ansible playbook using this role
3. ```docker run -d --name nextcloud-db -e MYSQL_ROOT_PASSWORD=supersecretpassword -v /data/mysql/data:/var/lib/mysql -v /data/mysql/conf.d:/etc/mysql/conf.d -e MYSQL_DATABASE=nextcloud -e MYSQL_USER=nextcloud -e MYSQL_PASSWORD=supersecretdatabasepassword mariadb:10.1```
4. ```docker run -d --name nextcloud-php -v /data/nextcloud/apps:/apps2 -v /data/nextcloud/config:/config -v /data/nextcloud/data:/data --link nextcloud-db:nextcloud-db -e ADMIN_USER=rlejolivet -e ADMIN_PASSWORD=verysecretpassword -e DB_TYPE=mysql -e DB_NAME=nextcloud -e DB_USER=nextcloud -e DB_PASSWORD=supersecretdatabasepassword -e DB_HOST=nextcloud-db wonderfall/nextcloud:11.0```
5. ```docker run -d --name nextcloud-proxy -p 80:80 -p 443:443 --link nextcloud-php:nextcloud -v /data/nginx/log:/var/log/nginx -v /data/nginx/conf.d:/etc/nginx/conf.d nginx:1.11```
6. Add "nextcloud:8888" to the "trusted_domaines" array
7. docker stop nextcloud-php
8. docker start nextcloud-php
