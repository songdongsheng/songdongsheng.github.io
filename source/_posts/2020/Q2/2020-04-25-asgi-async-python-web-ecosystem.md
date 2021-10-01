---
title: "ASGI: Async Python Web Ecosystem"
excerpt: "ASGI: Async Python Web Ecosystem"
date: 2020-04-25 21:39:00
tags:
  - Programming
  - Python
categories: [Programming, Python]
---

# ASGI: Async Python Web Ecosystem

There's a lot of exciting stuff happening in the Python web development ecosystem right now — one of the main drivers of this endeavor is [ASGI](https://github.com/django/asgiref/blob/master/specs/asgi.rst) , the **Asynchronous Server Gateway Interface**.

## An overview of ASGI

ASGI consists of two different components:

- A **protocol server**, which terminates sockets and translates them into connections and per-connection event messages.
- An **application**, which lives inside a **protocol server**, is called once per connection, and handles event messages as they happen, emitting its own event messages back when necessary.

There are two separate parts to an ASGI connection:

- A **connection scope**, which represents a protocol connection to a user and survives until the connection closes.
- **Events**, which are messages sent to the application as things happen on the connection, and messages sent back by the application to be received by the server, including data to be transmitted to the client.

## The advantages of ASGI

There are many more advantages to using ASGI-based components for building Python web apps.

- Speed: the async nature of ASGI apps and servers make them really fast (for Python, at least) — we're talking about 60k-70k req/s (consider that Flask and Django only achieve 10-20k in a similar situation).
- Features: ASGI servers and frameworks gives you access to inherently concurrent features (WebSocket, Server-Sent Events, HTTP/2) that are impossible to implement using sync/WSGI.
- Stability: ASGI as a spec has been around for about 3 years now, and version 3.0 is considered very stable. Foundational parts of the ecosystem are stabilizing as a result.

In terms of libraries and tooling, I don't think we can say we're there just yet. But thanks to a very active community, I have strong hopes that the ASGI ecosystem reaches feature parity with the traditional sync/WSGI ecosystem real soon.

## ASGI protocol servers

- Hypercorn - Python 3.7+
- Daphne - Python 3.5+
- Uvicorn - Python 3.6+

### Hypercorn

[Hypercorn] is an [ASGI](https://github.com/django/asgiref/blob/master/specs/asgi.rst) web server based on the sans-io hyper, [h11](https://github.com/python-hyper/h11), [h2](https://github.com/python-hyper/hyper-h2), and [wsproto](https://github.com/python-hyper/wsproto) libraries and inspired by Gunicorn. Hypercorn supports HTTP/1, **HTTP/2**, WebSockets (over HTTP/1 and HTTP/2), ASGI/2, and ASGI/3 specifications. Hypercorn can utilise asyncio, uvloop, or trio worker types.

Hypercorn was initially part of [Quart](https://gitlab.com/pgjones/quart) before being separated out into a standalone ASGI server. Hypercorn forked from version 0.5.0 of Quart.

```shell
pip install -U Hypercorn
```

### Daphne

[Daphne](https://github.com/django/daphne/) is a HTTP, **HTTP2** and WebSocket protocol server for [ASGI](https://github.com/django/asgiref/blob/master/specs/asgi.rst) and [ASGI-HTTP](https://github.com/django/asgiref/blob/master/specs/www.rst), developed to power [Django](https://github.com/django/django) [Channels](https://github.com/django/channels).

```shell
pip install -U daphne Twisted[tls,http2]
```

### Uvicorn

[Uvicorn](https://www.uvicorn.org/) is a lightning-fast ASGI server, built on uvloop and httptools.

[Uvicorn](https://www.uvicorn.org/) currently supports **_HTTP/1.1 and WebSockets_**. Support for ~~HTTP/2 is planned~~.

```shell
pip install -U uvicorn
```

## ASGI frameworks

- Quart - 659
- BlackSheep - 4
- FastAPI
- Sanic - 3168
- Starlette - 4824
- Django Channels - 7161

### Quart

[Quart](https://gitlab.com/pgjones/quart) is a Python [ASGI](https://github.com/django/asgiref/blob/master/specs/asgi.rst) web microframework. It is intended to provide the easiest way to use asyncio functionality in a web context, especially with existing Flask apps. This is possible as the Quart API is a superset of the [Flask](https://github.com/pallets/flask) API.

Quart aims to be a complete web microframework, as it supports HTTP/1.1, HTTP/2 and websockets. Quart is very extendable and has a number of known [extensions](https://pgjones.gitlab.io/quart/how_to_guides/quart_extensions.html) and works with many of the [Flask extensions](https://pgjones.gitlab.io/quart/how_to_guides/flask_extensions.html).

Also see this [cheatsheet](https://pgjones.gitlab.io/quart/reference/cheatsheet.html). To deploy in a production setting see the [deployment](https://pgjones.gitlab.io/quart/tutorials/deployment.html) documentation.

```shell
pip install -U Quart
```

### BlackSheep

[BlackSheep](https://github.com/RobertoPrevato/BlackSheep) is an asynchronous web framework to build event based, non-blocking Python web applications.

[BlackSheep](https://github.com/RobertoPrevato/BlackSheep) belongs to the category of [ASGI](https://asgi.readthedocs.io/en/latest/) web frameworks, so it requires an ASGI HTTP server to run, such as [uvicorn](http://www.uvicorn.org/), [daphne](https://github.com/django/daphne/), or [hypercorn](https://pgjones.gitlab.io/hypercorn/).

```shell
pip install -U blacksheep hypercorn
```

### FastAPI

[FastAPI](https://github.com/tiangolo/fastapi) is a modern, fast (high-performance), web framework for building APIs with Python 3.6+ based on standard Python type hints.

```shell
pip install -U fastapi hypercorn
```

### Sanic

[Sanic](https://github.com/huge-success/sanic) is a Python 3.6+ web server and web framework that's written to go fast. It allows the usage of the async/await syntax, which makes your code non-blocking and speedy.

[Sanic](https://github.com/huge-success/sanic) has three serving options: the inbuilt webserver, an [ASGI webserver](https://asgi.readthedocs.io/en/latest/implementations.html), or gunicorn.

```shell
pip install -U sanic hypercorn
```

### Starlette

[Starlette](https://github.com/encode/starlette) is a lightweight [ASGI](https://asgi.readthedocs.io/en/latest/) framework/toolkit, which is ideal for building high performance asyncio services.

```shell
pip install -U starlette hypercorn
```

### Django Channels

[Channels](https://github.com/django/channels) augments [Django](https://github.com/django/django) to bring WebSocket, long-poll HTTP, task offloading and other async support to your code, using familiar Django design patterns and a flexible underlying framework that lets you not only customize behaviours but also write support for your own protocols and needs.

[Django REST framework](https://www.django-rest-framework.org/) is a powerful and flexible toolkit for building Web APIs.

- The Web browsable API is a huge usability win for your developers.
- Authentication policies including packages for OAuth1a and OAuth2.
- Serialization that supports both ORM and non-ORM data sources.
- Customizable all the way down - just use regular function-based views if you don't need the more powerful features.
- Extensive documentation, and great community support.
- Used and trusted by internationally recognised companies including Mozilla, Red Hat, Heroku, and Eventbrite.

```shell
# python -m pip install -U Django
# python -m django --version
# python -c "import django; print(django.get_version())"
# python -c "import channels; print(channels.__version__)"
pip install -U Django channels djangorestframework django-filter markdown
```
