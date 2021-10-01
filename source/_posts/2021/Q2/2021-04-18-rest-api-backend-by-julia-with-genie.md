---
title: REST API backend by Julia with Genie
excerpt: REST API backend by Julia with Genie
date: 2021-04-18 23:57:36
tags:
    - Programming
    - Julia
categories: [Programming, Julia]
---

# REST API backend by Julia with Genie

## Introduction

### Julia language

Put the motto to rest.

> Julia Walks like Python, Runs like C.

Julia is a rather new and ground-breaking programming language that was released in early stages back in 2012. In that short span of time between then and 2020, the size of the ecosystem and the build of the language has absolutely exploded. Just looking at the popularity based on Github stars and Insights of the programming language, it is easy to see just how much the language has accomplished in just the past few years.

Julia’s success has ultimately correlated with the rapid machine-learning growth that the scientific ecosystem has experienced in general.

### Genie web framework

Genie is a full-stack MVC web framework that provides a streamlined and efficient workflow for developing modern web applications. It builds on Julia's strengths (high-level, high-performance, dynamic, JIT compiled), exposing a rich API and a powerful toolset for productive web development.

## How to Install Genie

### By interactive environment

```julia
julia> import Pkg; Pkg.add("Genie")
```

### By Pkg REPL - from Julia's registry
```julia
pkg> add Genie
```

### By Pkg REPL - by running off the master branch

```julia
pkg> add Genie#master
```

## Developing Genie Web Services

### REST API backend - coding

```julia
using Genie, Genie.Router, Genie.Renderer.Json, Genie.Requests
using HTTP

route("/") do
  "Welcome to Genie!" |> json
end

route("/foo") do
  json(:foo => "Foo")
end

route("/sum/:x::Int/:y::Int") do
    params(:x) + params(:y) |> json
end

Genie.startup(port = 8080, async = false)
```

### REST API backend - running

```shell
julia rest-demo.jl
┌ Info:
└ Web Server starting at http://127.0.0.1:8080 - press Ctrl/Cmd+C to stop the server.
```

### REST API client

```shell
$ curl -isS --http2 http://127.0.0.1:8080/

HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Server: Genie/Julia/1.5.4
Transfer-Encoding: chunked

"Welcome to Genie!"
```

```shell
$ curl -isS --http2 http://127.0.0.1:8080/foo
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Server: Genie/Julia/1.5.4
Transfer-Encoding: chunked

{"foo":"Foo"}
```

```shell
$ curl -isS --http2 http://127.0.0.1:8080/sum/3/8
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Server: Genie/Julia/1.5.4
Transfer-Encoding: chunked

11
```
