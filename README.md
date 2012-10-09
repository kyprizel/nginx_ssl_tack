Description
===========

This patch enables the NGINX SSL module to respond with a [TACK](http://tack.io/) TLS Extension.


Directives
==========

ssl_tack
--------
**syntax:** *ssl_tack (on|off);*

**default:** *off*

**context:** *server*

on - Enable TLS extension

off - Disable TLS extension


ssl_tack_file
-------------
**syntax:** *ssl_tack_file &lt;string&gt;*

**default:** *none*

**context:** *server*

Sets TACK file path.


ssl_tack_pin_activation
-----------------------
**syntax:** *ssl_tack_pin_activation (on|off);*

**default:** *off*

**context:** *server*

on - Enable TACK pin activation

off - Disable TACK pin activation


Installation
============

Grab the nginx source code from [nginx.org](<http://nginx.org/>).
Patch and compile it with [openssl_tack](https://github.com/tack/openssl_tack).

    wget 'http://nginx.org/download/nginx-VERSION.tar.gz'
    tar -xzvf nginx-VERSION.tar.gz
    patch -p0 < ngx_http_ssl_module-VERSION.patch

    ./configure --with-debug --with-openssl=/path/to/openssl_tack --with-http_ssl_module
    make
    make install


Example configuration
=====================

    server {
        listen       443;
        server_name  localhost;

        ssl                  on;
        ssl_certificate      ssl/testhost.crt;
        ssl_certificate_key  ssl/testhost.key;

        ssl_session_timeout  5m;

        ssl_protocols  SSLv2 SSLv3 TLSv1;
        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers   on;

        ssl_tack on;
        ssl_tack_file ssl/tack.sig;
        ssl_tack_pin_activation off;

        location / {
            root   html;
        }
    }
