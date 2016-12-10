# ansible-nextcloud
Ansible role to install a docker containerized Nextcloud instance

Components :
- [wonderfall/nextcloud docker image](https://hub.docker.com/r/wonderfall/nextcloud/)
- [Official mariadb docker image](https://hub.docker.com/_/mariadb/)
- [Official nginx docker image](https://hub.docker.com/_/nginx/)
- [Minimal Let's Encrypt webroot docker image](https://github.com/RLejolivet/docker-letsencrypt-webroot)

Assumes the target machine does not have any web server.

Any firewall should open ports 80 (for Let's Encrypt verification process) and 443 (HTTPS access to the Nextcloud instance)
