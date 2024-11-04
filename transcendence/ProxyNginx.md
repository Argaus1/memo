# PROXY AND NGINX

## Proxy and reverse proxy

The client/browser doesn't communicate directly with the server, the proxy does it. 
Hides the client IP, only the proxy knows it.
Cache for most visited pages. 

Reverse proxy : 

- protects the web server. Intercepts the http requests on behalf of the web server.

- load balancing ? in case of a lot of requests

- RP caches static content. If a piece of content is requested again, locally cached version can be returned.

- 
