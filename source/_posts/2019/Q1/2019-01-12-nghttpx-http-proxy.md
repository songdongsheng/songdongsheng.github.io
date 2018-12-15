---
title: nghttpx http proxy
description: nghttpx http proxy
date: 2019-01-12 23:49:26
tags:
  - Programming
  - HTTP
categories: [Programming, HTTP]
permalink: nghttpx-http-proxy
---

# nghttpx http proxy

- https://nghttp2.org/documentation/nghttpx-howto.html

## TLS encryption

1. The frontend connections are encrypted with SSL/TLS by default.

        frontend=*,443
        frontend=*,80;no-tls

2. The TLS encryption is now disabled on backend connection in all modes by default.

        backend=127.0.0.1,8080;tls

## HTTP protocol

1. The default backend protocol is HTTP/1.1
2. HTTP/2 and HTTP/1 are available on the frontend, and an HTTP/1 connection can be upgraded to HTTP/2 using HTTP Upgrade. Starting HTTP/2 connection by sending HTTP/2 connection preface is also supported.

## Session affinity

Two kinds of session affinity are available: client IP, and HTTP Cookie.

To enable client IP based affinity, specify **affinity=ip** parameter in **--backend** option.

    backend=127.0.0.1,3000;;affinity=ip

To enable HTTP Cookie based affinity, specify **affinity=cookie** parameter, and specify a name of cookie in **affinity-cookie-name** parameter. Optionally, a Path attribute can be specified in **affinity-cookie-path** parameter:

    backend=127.0.0.1,3000;;affinity=cookie;affinity-cookie-name=nghttpxlb;affinity-cookie-path=/

## HTTP proxy

### proxy patterns

nghttpx can route request to different backend according to request host and path.

    # they are used as load balancing
    backend=192.168.0.10,8080
    backend=192.168.0.11,8008

    backend=docserv,3000;doc.example.com/
    backend=::1,8080;/bar/
    backend=::1,8080;/sample*
    backend=192.168.0.10,8080;example.com/foo

    # they are used as load balancing
    backend=serv1,3000;example.com/myservice
    backend=serv2,3000;example.com/myservice

    backend=serv1,3000;/;proto=h2
    backend=serv1,3000;/ws/;proto=http/1.1

    backend=serv1,8443;/;proto=h2;tls
    backend=serv2,8080;/ws/;proto=http/1.1

### catch all pattern

One important thing you have to remember is that we have to specify default routing pattern for so called "catch all" pattern. To write "catch all" pattern, just specify backend server address, without pattern.

Usually, host is the value of Host header field. In HTTP/2, the value of :authority pseudo header field is used.
