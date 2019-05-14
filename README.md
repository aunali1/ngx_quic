ngx_quic
========

An experimental implementaion of the QUIC protocol for NGINX.

# Documentation
ATTENTION: This module does not enable QUIC on your server for real. It is just an experimental implementation that can be enabled on a specific location.

## Installation / Compilation
The common module installation is always as follows in nginx:
`--add-module="dir/to/modules/ngx_quic`

The above line must be added as an argument when you run the `./configure`. More on this can be found on https://www.nginx.com/resources/wiki/start/topics/tutorials/installoptions/ .
-----
## Configuration
check [line 17 in ngx_quic_module_c](https://github.com/aunali1/ngx_quic/blob/95ace332b7587fd560e86c762c23a328c3301732/src/ngx_quic_module.c#L17):
```
// This directive may be specified in the location-level of your nginx-config.
// The directive does not take any arguments [NGX_CONF_NOARGS]]
NGX_HTTP_LOC_CONF|NGX_CONF_NOARGS,
```

What this means is, that your `nginx.conf` must look like this:

```
...
http {
    ....
    server {
        listen        0.0.0.0:443  sssl http2;
        server_name   one.example.com  www.one.example.com;

        location / {
            quic;
        }
    }
}
```
of course, you'll have to replace some of the above accordingly, e.g. IP address should be the IP of  your server

Configuring this will result in:

```sh
$ curl https://one.example.com/
Experimental QUIC Implementation for NGINX - Aun-Ali Zaidi
```
-----
## Notes:
But be aware, this module does not really enable a QUIC support on your server. All it does is to get the HTTP request and respind it with a predefined response using QUIC and it will always respond with the same string. There seems to be no QUIC implementation for nginx yet, though there are other server software to have implemented it (e.g. litespeed) and available libraries for building your own modules or server (e.g. [Cloudflare Quiche](https://github.com/cloudflare/quiche), [Facebook mvzft](https://github.com/facebookincubator/mvfst), etc.)
