# PROXY AND NGINX

[Nginx-WebServerDoc](https://docs.nginx.com/nginx/admin-guide/web-server/)

[Nginx-ContentCaching](https://docs.nginx.com/nginx/admin-guide/content-cache/content-caching/)

[Nginx-LoadBalancing](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/)

## Proxy and reverse proxy

The client/browser doesn't communicate directly with the server, the proxy does it. 
Hides the client IP, only the proxy knows it.
Cache for most visited pages. 

Reverse proxy : 

- protects the web server. Intercepts the http requests on behalf of the web server. add an authentication

- load balancing ? in case of a lot of requests. rout requests.

- RP caches static content. If a piece of content is requested again, locally cached version can be returned. (not useful in our SPA case ?)


## Nginx reverse proxy

> It is possible to proxy requests to an HTTP server (another NGINX server or any other server) or a non-HTTP server (which can run an application developed with a specific framework, such as PHP or Python) using a specified protocol. Supported protocols include FastCGI, uwsgi, SCGI, and memcached.

= case we're interested in. FastCGI for example is a protocol.


