---
title: Docker authentication information in the Kubernetes secret
description: Docker login information in the Kubernetes secret
date: 2019-02-05 23:20:59
tags:
  - Kubernetes
categories: [Cloud, Kubernetes]
permalink: kubernetes-secret-docker-auths
---

# Docker authentication information in the Kubernetes secret

When pull an image from a private registry, we need put docker config.json file in the Kubernetes secret, and use it by Kubernetes imagePullSecrets.

## Docker config.json

The docker login process creates or updates a config.json file that holds an authorization token.

```yaml
# docker login
# cat ~/.docker/config.json
{
    "auths": {
        "https://index.docker.io/v1/": {
            "auth": "c3R...zE2"
        },
        ...
    }
}
```

## Create Kubernetes secret via docker config.json

```shell
kubectl create secret generic armdocker \
    --from-file=.dockerconfigjson=config.json \
    --type=kubernetes.io/dockerconfigjson
```

## Create Kubernetes secret via docker username and password

```shell
kubectl create secret docker-registry armdocker \
    --docker-server=<your-registry-server> \
    --docker-username=<your-name> \
    --docker-password=<your-pword>
```

## Inspecting the Secret

```yaml
# kubectl get secret armdocker --output=yaml

    apiVersion: v1
    kind: Secret
    metadata:
      ...
      name: armdocker
      ...
    data:
      .dockerconfigjson: eyJodHRwczovL2luZGV4L ... J0QUl6RTIifX0=
    type: kubernetes.io/dockerconfigjson

# kubectl get secret armdocker --output="jsonpath={.data.\.dockerconfigjson}" | base64 --decode| jq

    {
      "auths": {
        "your.private.registry.example.com": {
          "username": "janedoe",
          "password": "xxxxxxxxxxx",
          "email": "jdoe@example.com",
          "auth": "c3R...zE2"
        }
      }
    }
```

## Use Kubernetes secret of docker registry

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: sample-of-docker-registry-secret
spec:
  template:
    spec:
      restartPolicy: Always
      imagePullSecrets:
        - name: armdocker
      initContainers:
        - name: wait-for-precondition
          image： <your-private-image>
          imagePullPolicy: Always
      containers:
        - name: sample-service-name
          image: <your-private-image>
          imagePullPolicy: Always
```