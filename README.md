yamlNginx
=====

[![Build Status](https://travis-ci.org/jebovic/ansible-nginx.svg?branch=master)](https://travis-ci.org/jebovic/ansible-nginx) [![Ansible Galaxy](https://img.shields.io/badge/galaxy-jebovic.nginx-blue.svg?style=flat)](https://galaxy.ansible.com/jebovic/nginx)

Install and configure Nginx, add your own virtualhosts from yaml variables

This role is a part of my [OPS project](https://github.com/jebovic/ops), follow this link to see it in action. OPS provides a lot of stuff, like a vagrant file for development VMs, playbooks for roles orchestration, inventory files, examples for roles configuration, ansible configuration file, and many more.

Compatibility
-------------

Tested and approved on :

* Debian jessie (8+)
* Ubuntu Trusty (14.04 LTS)
* Ubuntu Xenial (16.04 LTS)

Role Variables
--------------

```yaml
# Nginx install configuration
nginx_apt_key_url: https://nginx.org/keys/nginx_signing.key
nginx_apt_repositories:
  - "deb http://nginx.org/packages/{{ ansible_distribution | lower }}/ {{ ansible_distribution_release | lower }} nginx"
  - "deb-src http://nginx.org/packages/{{ ansible_distribution | lower }}/ {{ ansible_distribution_release | lower }} nginx"
nginx_packages:
  - nginx

# Nginx basic configuration
nginx_config_path: /etc/nginx
nginx_user: www-data
nginx_user_group: www-data
nginx_port: 80
nginx_process_number: 4
nginx_pid_file: /var/run/nginx.pid
nginx_worker_connections: 768
nginx_default_access_log: /var/log/nginx/access.log
nginx_default_error_log: /var/log/nginx/error.log
nginx_default_root: /var/www
nginx_default_host: "{{ ansible_host }}"
nginx_default_index: index.php index.html;
nginx_includes:
  - "{{ nginx_config_path }}/conf.d/*.conf"

# Add your own vhosts, simply override nginx_custom_vhosts to fit your needs
nginx_default_vhost_enabled: yes
nginx_custom_vhosts:
  - port: "{{ nginx_port }}"
    server_name: "{{ ansible_host }}"
    root: "{{ nginx_default_root }}"
    access_log: /var/log/nginx/default.access.log
    error_log: /var/log/nginx/default.error.log
    active_php: yes
    front_controller: index
    index: index.html
    strip_prefix: no
    rewrites: no
    misc: no

# Specific link with php fpm
php_fpm_socket_path: /var/run/php-fpm.sock
```

Example Playbook
----------------

```yaml
- hosts: servers
  roles:
     - { role: jebovic.nginx }
```

Example : config
----------------

```yaml
# configure your own vhosts, see templates/conf.d/custom_vhost.conf for more information
# fully functionnal and simple sulu (CMS on top of Symfony PHP Framework) vhost below
nginx_custom_vhosts:
  - port: 443
    server_name: sulu.local
    root: /srv/www/sulu/web
    access_log: /var/log/nginx/sulu.local.access.log
    error_log: /var/log/nginx/sulu.local.error.log
    strip_prefix: app\.php
    active_php: yes
    front_controller: website|admin|app|app_dev|config
    index: no
    rewrites:
      - name: admin
        location: /admin
        index: admin.php
      - name: website
        location: /
        index: website.php
```

Tags
----

* nginx_config : only update config and restart service

License
-------

MIT

Author Information
------------------

Jérémy Baumgarth https://github.com/jebovic
