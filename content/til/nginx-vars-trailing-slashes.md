---
title: "Nginx, apache and trailing slashes"
date: 2025-03-02
description: "Nginx and Apache reverse proxying behaviours"
tags: ["til", "nginx", "apache", "proxy"]
---
I've used combinations of [nginx](https://nginx.org/) and [apache](https://httpd.apache.org/) 
at work for almost 15 years now, but I don't think I've ever had to migrate configuration from one to the other.

This week I lost a few hours by a surprising and subtle difference in how the `proxypass` directive works in nginx.

Here's a docker-compose.yml that starts two nginx servers, one proxying requests to another:
```yaml
---
services:
  nginx-proxy:
    image: nginx:latest
    container_name: proxy
    ports:
      - "8080:80"
    volumes:
      - ./proxy.conf:/etc/nginx/nginx.conf
    depends_on:
      - nginx-backend

  nginx-backend:
    container_name: backend
    image: nginx:latest
    ports:
      - "8081:80"
    volumes:
      - ./backend.conf:/etc/nginx/nginx.conf
```

The config for the proxy looks like this:
```
events {}
http {
    server {
        listen 80;
        # proxy_pass without a trailing slash 
        location /novar/notrailing/ {
            proxy_pass http://backend:8081;
        }
        # proxy_pass with a trailing slash
        location /novar/trailing/ {
            proxy_pass http://backend:8081/;
        }
        # proxy_pass using a variable to set the upstream host, without a trailing slash
        location /var/notrailing/ {
            set $upstream http://backend:8081;
            proxy_pass $upstream;
        }
        # proxy_pass using a variable to set the upstream host, with a trailing slash
        location /var/trailing/ {
            set $upstream http://backend:8081/;
            proxy_pass $upstream;
        }
    }
}
```

And the config for the backend is below. It's configured to return a 200 for any request
and the request URI it received from the proxy.

```
events {}
http {
    server {
        listen 8081;
        location / {
            add_header Content-Type text/plain;
            return 200 "$request";
        }
    }
}
```

Starting the servers:
```
docker compose up -d
```
Now to compare the behaviour when using variables and trailing slashes in the proxy_pass directive.

### No variable, no trailing slash

Without a trailing slash, nginx proxies the full request URI through to the backend: 

```
❯ curl localhost:8080/novar/notrailing/test/
/novar/notrailing/test/
```

### No variable, with a trailing slash

With a trailing slash, nginx removes the matching part of the URI (/novar/trailing) and passes through the rest (/test/)
```
❯ curl localhost:8080/novar/trailing/test/
/test/
```

### Using a variable for the host, without a trailing slash

One of the reasons to use a variable when setting the upstream host in the proxy_pass directive is to
stop nginx attempting to resolve the domain at startup. But this causes a subtle change in behaviour.

First off, with no trailing slash, everything works as before and the whole URI is passed through:
```
❯ curl localhost:8080/var/notrailing/test/
/var/notrailing/test/
```

### Using a variable for the host, with a trailing slash

With a trailing slash though, none of the URI is passed through!
```
❯ curl localhost:8080/var/trailing/test/
/
```
