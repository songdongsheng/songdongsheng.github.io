---
title: Running Ceph inside Docker
description: Containerize Ceph
date: 2018-12-15 22:10:48
tags:
  - Cloud
  - Container
categories: [Cloud, Container]
permalink: running-ceph-inside-docker
---

# Running Ceph inside Docker

## Ceph Versions and Docker image tags
- https://docs.ceph.com/docs/master/releases/schedule/
- https://docs.ceph.com/docs/master/start/kube-helm/
- https://hub.docker.com/r/ceph/daemon/tags
- https://hub.docker.com/r/ceph/ceph/tags

## Find available container image tags

    curl -sSL https://registry.hub.docker.com/v2/repositories/ceph/daemon/tags/ | jq '."results"[] .name'
    curl -sSL https://registry.hub.docker.com/v2/repositories/ceph/daemon/tags/?page=2 | jq '."results"[] .name'

## Deploy a monitor

    docker run -d --net=host \
    -v /etc/ceph:/etc/ceph \
    -v /var/lib/ceph/:/var/lib/ceph/ \
    -e MON_IP=192.168.5.120 \
    -e CEPH_PUBLIC_NETWORK=192.168.5.0/24 \
    ceph/daemon mon

## Deploy a Manager daemon

    docker run -d --net=host \
    -v /etc/ceph:/etc/ceph \
    -v /var/lib/ceph/:/var/lib/ceph/ \
    ceph/daemon mgr

## Deploy an OSD

    docker run -d --net=host \
    --pid=host \
    --privileged=true \
    -v /etc/ceph:/etc/ceph \
    -v /var/lib/ceph/:/var/lib/ceph/ \
    -v /dev/:/dev/ \
    -e OSD_DEVICE=/dev/vdd \
    -e OSD_TYPE=disk \
    -e OSD_BLUESTORE=1 \
    ceph/daemon osd

## Deploy a MDS

    docker run -d --net=host \
    -v /var/lib/ceph/:/var/lib/ceph/ \
    -v /etc/ceph:/etc/ceph \
    -e CEPHFS_CREATE=1 \
    ceph/daemon mds

## Deploy a Rados Gateway

    docker run -d --net=host \
    -v /var/lib/ceph/:/var/lib/ceph/ \
    -v /etc/ceph:/etc/ceph \
    ceph/daemon rgw
