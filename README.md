Nginx
=====

Install and configure Nginx, add your own virtualhosts from yaml variables

Role Variables
--------------

```
# Nginx basic configuration
nginx_user: www-data
nginx_port: 80
nginx_process_number: 4
nginx_pid_file: /var/run/nginx.pid
nginx_worker_connections: 768
nginx_default_access_log: /var/log/nginx/access.log
nginx_default_error_log: /var/log/nginx/error.log
nginx_default_root: /srv/www
nginx_default_host: "{{ ansible_host }}"
nginx_default_index: index.php index.html;
nginx_includes:
  - /etc/nginx/conf.d/*.conf

# Add your own vhosts, simply override nginx_custom_vhosts to fit your needs
nginx_custom_vhosts:
  - port: "{{ nginx_port }}"
    server_name: "{{ ansible_host }}"
    root: "{{ nginx_default_root }}"
    access_log: /var/log/nginx/default.access.log
    error_log: /var/log/nginx/default.error.log
    active_php: true
    front_controller: index
    index: index.php
    strip_prefix: false
    rewrites: false

# Specific link with php fpm
php_fpm_socket_path: /var/run/php5-fpm.sock
```

Example Playbook
----------------

```
    - hosts: servers
      roles:
         - { role: jebovic.nginx }
```

License
-------

MIT

Author Information
------------------

Jérémy Baumgarth https://github.com/jebovic
