---
title: Helm Template random value
description: Helm Template random value
date: 2019-01-05 21:37:08
tags:
  - Cloud
  - Kubernetes
categories: [Cloud, Kubernetes]
permalink: helm-template-random-value
---

# Helm Template random value

## Sprig Function Documentation

The Sprig library provides over 70 template functions for Go’s template language.

- http://masterminds.github.io/sprig/


## Random functions

These four functions generate cryptographically secure random strings, but with different base character sets:

- randAlphaNum uses 0-9a-zA-Z
- randAlpha uses a-zA-Z
- randNumeric uses 0-9
- randAscii uses all printable ASCII characters

Each of them takes one parameter: the integer length of the string.
e.g. This will produce a random string with 12 ASCII characters.

    randAscii 12

## Using in the Helm template

### values.yaml

    data_with_rand_value: 'abcd-{{ randAlphaNum 12 }}'

### templates/

    data_with_rand_value: {{ tpl .Values.data_with_rand_value . | quote }}

### random result

    data_with_rand_value: "abcd-jXf7OWkGpRH2"
