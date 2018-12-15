---
title: How to set up a WebDAV share with Nginx
description: How to set up a WebDAV share with Nginx
date: 2020-03-01 21:59:57
tags:
  - Linux
categories: [Operating system, Linux]
permalink: nginx-webdav
---

# How to set up a WebDAV share with Nginx

## Built-in WebDAV Module

The **ngx\_http\_dav\_module** module is intended for file management automation via the **WebDAV** protocol. The module processes HTTP and WebDAV methods **PUT**, **DELETE**, **MKCOL**, **COPY**, and **MOVE**.

**WebDAV clients that require additional WebDAV methods to operate will not work with this module.** Currently, only **hdav** of [Haskell DAV library](https://salsa.debian.org/clint/DAV) works.

### nginx-dav.conf

```nginx
location /dav {
    root /var;

    auth_basic              WebDAV;
    auth_basic_user_file    /etc/nginx/.htpasswd;

    dav_access user:rw group:rw all:r;
    dav_methods PUT DELETE MKCOL COPY MOVE;

    client_max_body_size    0;
    create_full_put_path    on;
    client_body_temp_path   /var/cache/nginx/client_temp 1 2 3;
}
```

### hdav

```shell
sudo apt-get install -y --no-install-recommends hdav

hdav put testfile https://example.com/dav/testfile \
    --username "${username}" --password "${password}"
```

## WebDAV missing methods support for Nginx

Since both [**cadaver**](http://www.webdav.org/cadaver/) and [**davfs2**](http://savannah.nongnu.org/projects/davfs2) need methods **OPTIONS** and **PROPFIND**, we need [**ngx\_dav\_ext\_module**](https://github.com/arut/nginx-dav-ext-module) module for these missing methods.

### nginx-dav-ext.conf

```nginx
# load_module modules/ngx_http_dav_ext_module.so;
# dav_ext_lock_zone zone=DAV:8m;
# dav_ext_lock zone=DAV;

location /dav {
    root /var;

    auth_basic              WebDav;
    auth_basic_user_file    /etc/nginx/.htpasswd;

    dav_access user:rw group:rw all:r;
    dav_methods PUT DELETE MKCOL COPY MOVE;
    dav_ext_methods PROPFIND OPTIONS LOCK UNLOCK;

    client_max_body_size    0;
    create_full_put_path    on;
    client_body_temp_path   /var/cache/nginx/client_temp 1 2 3;
}
```

### cadaver and davfs2

```shell
sudo apt-get install -y --no-install-recommends cadaver davfs2

sudo apt-get install -y --no-install-recommends -t testing nginx libnginx-mod-http-dav-ext

cadaver https://example.com/dav/

sudo mount -t davfs https://example.com/dav/ /mnt/

"DELETE /dav/xyz/ HTTP/1.1" 204 0 "-" "davfs2/1.5.5 neon/0.30.2"
"DELETE /dav/xyz/ HTTP/1.1" 204 0 "-" "cadaver/0.23.3 neon/0.30.2"
```

## Windows 10 WebClient

Add support **http** in addition to **https**:

```
regedit HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WebClient\Parameters
    BasicAuthLevel=2

net stop webclient
net start webclient
```

WebClient not good as **hdav** of [Haskell DAV library](https://salsa.debian.org/clint/DAV), [**cadaver**](http://www.webdav.org/cadaver/) or [**davfs2**](http://savannah.nongnu.org/projects/davfs2). It has a lot of annoying problems and looks like a half-finished product.
