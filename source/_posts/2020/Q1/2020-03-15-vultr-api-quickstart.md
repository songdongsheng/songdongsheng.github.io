---
title: Vultr API Quickstart
description: Vultr API Quickstart
date: 2020-03-15 20:16:51
tags:
  - Cloud
  - REST
  - Programming
categories: [Programming, API]
permalink: vultr-api-quickstart
---

# Vultr API Quickstart

## Overview

- API Endpoint: https://api.vultr.com/
- Available in [Members Area](https://my.vultr.com/settings/#settingsapi)
- Authentication: For any API request that requires authentication, you would need to send the 'API-Key: YOURKEY' HTTP header. See the cURL examples below for more information on how to do this.
- Time and Date: All time and date fields returned by this API are displayed in UTC.
- HTTP Response Codes:

    HTTP Response Code | Description
    -------------------|------------
    200                | Function successfully executed.
    400                | Invalid API location. Check the URL that you are using.
    403                | Invalid or missing API key. Check that your API key is present and matches your assigned key.
    405                | Invalid HTTP method. Check that the method (POST|GET) matches what the documentation indicates.
    412                | Request failed. Check the response body for a more detailed description.
    500                | Internal server error. Try again at a later time.
    503                | Rate limit hit. API requests are limited to an average of 2/s. Try your request again later.

## Retrieve a list of all active regions

```shell
# curl -sSL https://api.vultr.com/v1/regions/list | jq .
{
  "5": {
    "DCID": "5",
    "name": "Los Angeles",
    "country": "US",
    "continent": "North America",
    "state": "CA",
    "ddos_protection": true,
    "block_storage": false,
    "regioncode": "LAX"
  },
  "6": {
    "DCID": "6",
    "name": "Atlanta",
    "country": "US",
    "continent": "North America",
    "state": "GA",
    "ddos_protection": false,
    "block_storage": false,
    "regioncode": "ATL"
  },
  "19": {
    "DCID": "19",
    "name": "Sydney",
    "country": "AU",
    "continent": "Australia",
    "state": "",
    "ddos_protection": false,
    "block_storage": false,
    "regioncode": "SYD"
  }
}
```

## Retrieve a list of all active plans

```shell
# curl -sSL https://api.vultr.com/v1/plans/list_vc2 | jq .
{
  "201": {
    "VPSPLANID": "201",
    "name": "1024 MB RAM,25 GB SSD,1.00 TB BW",
    "vcpu_count": "1",
    "ram": "1024",
    "disk": "25",
    "bandwidth": "1.00",
    "bandwidth_gb": "1024",
    "price_per_month": "5.00",
    "plan_type": "SSD"
  },
  "208": {
    "VPSPLANID": "208",
    "name": "98304 MB RAM,1600 GB SSD,15.00 TB BW",
    "vcpu_count": "24",
    "ram": "98304",
    "disk": "1600",
    "bandwidth": "15.00",
    "bandwidth_gb": "15360",
    "price_per_month": "640.00",
    "plan_type": "SSD"
  }
}

# curl -sSL https://api.vultr.com/v1/plans/list_vc2z | jq .
{
  "400": {
    "VPSPLANID": "400",
    "name": "1024 MB RAM,32 GB SSD,1.00 TB BW",
    "vcpu_count": "1",
    "ram": "1024",
    "disk": "32",
    "bandwidth": "1.00",
    "bandwidth_gb": "1024",
    "price_per_month": "6.00",
    "plan_type": "HIGHFREQUENCY"
  },
  "406": {
    "VPSPLANID": "406",
    "name": "49152 MB RAM,768 GB SSD,8.00 TB BW",
    "vcpu_count": "12",
    "ram": "49152",
    "disk": "768",
    "bandwidth": "8.00",
    "bandwidth_gb": "8192",
    "price_per_month": "256.00",
    "plan_type": "HIGHFREQUENCY"
  }
}

# curl -sSL "https://api.vultr.com/v1/plans/list?type=vc2z" | jq .
{
  "400": {
    "VPSPLANID": "400",
    "name": "1024 MB RAM,32 GB SSD,1.00 TB BW",
    "vcpu_count": "1",
    "ram": "1024",
    "disk": "32",
    "bandwidth": "1.00",
    "bandwidth_gb": "1024",
    "price_per_month": "6.00",
    "plan_type": "HIGHFREQUENCY",
    "available_locations": [1, 2, 3, 4, 5, 6, 7, 8, 9, 12, 19, 22, 24, 25, 39, 40]
  },
  "406": {
    "VPSPLANID": "406",
    "name": "49152 MB RAM,768 GB SSD,8.00 TB BW",
    "vcpu_count": "12",
    "ram": "49152",
    "disk": "768",
    "bandwidth": "8.00",
    "bandwidth_gb": "8192",
    "price_per_month": "256.00",
    "plan_type": "HIGHFREQUENCY",
    "available_locations": []
  }
}
```

## Retrieve a list of the VPSPLANIDs currently available in this location

```shell
# curl -sSL "https://api.vultr.com/v1/regions/availability?DCID=1" | jq .
[201, 202, 203, 204, 205, 206, 115, 116, 117, 400, 401, 402, 403, 404, 405, 29, 93, 94, 95, 96, 97, 98, 100]
```

## Retrieve a list of available operating systems

```shell
# curl -sSL https://api.vultr.com/v1/os/list | jq .
{
  "270": {
    "OSID": 270,
    "name": "Ubuntu 18.04 x64",
    "arch": "x64",
    "family": "ubuntu"
  },
  "327": {
    "OSID": 327,
    "name": "FreeBSD 12 x64",
    "arch": "x64",
    "family": "freebsd"
  },
  "352": {
    "OSID": 352,
    "name": "Debian 10 x64 (buster)",
    "arch": "x64",
    "family": "debian"
  },
  "362": {
    "OSID": 362,
    "name": "CentOS 8 x64",
    "arch": "x64",
    "family": "centos",
    "windows": false
  },
  "366": {
    "OSID": 366,
    "name": "OpenBSD 6.6 x64",
    "arch": "x64",
    "family": "openbsd"
  }
}
```

## Create a new virtual machine

```shell
# curl -sSL -H "API-Key: ${VULTR_API_KEY}" https://api.vultr.com/v1/server/create --data 'DCID=1' --data 'VPSPLANID=400' --data 'OSID=352'
{
  "SUBID": "1312965"
}
```

## List all active or pending virtual machines

```shell
# curl -sSL -H "API-Key: ${VULTR_API_KEY}" https://api.vultr.com/v1/server/list | jq .
{
  "576965": {
    "SUBID": "576965",
    "os": "Debian 10 x64 (buster)",
    "ram": "1024 MB",
    "disk": "Virtual 32 GB",
    "main_ip": "123.123.123.123",
    "vcpu_count": "1",
    "location": "Los Angeles",
    "DCID": "5",
    "default_password": "o6%$#6xf!ekUD",
    "date_created": "2013-12-19 14:45:41",
    "pending_charges": "0.18",
    "status": "active",
    "cost_per_month": "6.00",
    "current_bandwidth_gb": 6.58,
    "allowed_bandwidth_gb": "1000",
    "netmask_v4": "255.255.255.248",
    "gateway_v4": "123.123.123.1",
    "power_status": "running",
    "server_state": "ok",
    "VPSPLANID": "400",
    "v6_main_ip": "2001:DB8:1000::100",
    "v6_network_size": "64",
    "v6_network": "2001:DB8:1000::",
    "v6_networks": [
      {
        "v6_network": "2001:DB8:1000::",
        "v6_main_ip": "2001:DB8:1000::100",
        "v6_network_size": "64"
      }
    ],
    "label": "vultr-lax-ca-us-02",
    "internal_ip": "10.99.0.10",
    "kvm_url": "https://my.vultr.com/subs/vps/novnc/api.php?data=djJ8dngxTmFh-tT8qW56h",
    "auto_backups": "no",
    "tag": "mytag",
    "OSID": "352",
    "APPID": "0",
    "FIREWALLGROUPID": "0"
  }
}
```


## Destroy (delete) a virtual machine

```shell
# curl -sSL -i -H "API-Key: ${VULTR_API_KEY}" https://api.vultr.com/v1/server/destroy --data 'SUBID=1312965'

HTTP/2 412
content-type: text/html; charset=UTF-8

Invalid server.  Check SUBID value and ensure your API key matches the server's account
```
