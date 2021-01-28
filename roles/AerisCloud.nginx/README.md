nginx
=====

Install nginx on your server.
It doesn't setup any server block for you. 

Configuration
-------------

### Inventory

```ini
[web:children]
web-somedc-prod

[web-somedc-prod]
web1.somedc.prod         ansible_ssh_host=10.0.1.111

[web-somedc-prod:vars]
#
# Set the max number of connection nginx workers can handle (default: 4096)
#
nginx_max_connections = 10000

#
# Set the max number of nginx worker to run (default: 2)
#
nginx_worker_process = 2

#
# Set this to "on" if you want workers to accept
# more than one connection at a time (default: off)
#
nginx_multi_accept = off

#
# Set this to "on" if you want enable the use of sendfile
# (default: off)
#
nginx_sendfile = off

#
# tcp_nopush tries to optimize the amount of data sent at once
# (default: off)
#
# See https://t37.net/nginx-optimization-understanding-sendfile-tcp_nodelay-and-tcp_nopush.html
# for more details
#
nginx_tcp_nopush: off

#
# Use tcp_nodelay to disable the use of Nagle's algorithm
# (default: off)
#
# See https://t37.net/nginx-optimization-understanding-sendfile-tcp_nodelay-and-tcp_nopush.html
# for more details
#
nginx_tcp_nodelay: off

#
# Keepalive duration (default: 30)
#
nginx_keepalive_timeout = 30

#
# Set this to "on" if you want nginx to gzip what
# it distributes (default: off)
#
nginx_gzip = off

# Set it to true to add the SSL configuration
nginx_use_ssl = false

[... skipping ...]

[somedc-prod:children]
web-somedc-prod
```

SSL
---

If you enable the SSL, you must add the following configuration in your server block configuration.
```
{% if nginx_use_ssl|bool %}
    listen 443 ssl;

    # certs sent to the client in SERVER HELLO are concatenated in ssl_certificate
    ssl_certificate /etc/nginx/ssl/bundle.crt;
    ssl_certificate_key /etc/nginx/ssl/server.key;

    ## verify chain of trust of OCSP response using Root CA and Intermediate certs
    ssl_trusted_certificate /etc/nginx/ssl/trusted.crt;
{% endif %}
```

You need the following files:
* `/etc/nginx/ssl/bundle.crt`, which contains your certificate and the intermediate certificates.
* `/etc/nginx/ssl/server.key`, which is your private key.
* `/etc/nginx/ssl/trusted.crt`, which contains the intermediate certificates and the root certificate.
