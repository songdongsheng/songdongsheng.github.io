---
title: Speed up apt installation or upgrade
excerpt: Speed up apt installation or upgrade
date: 2020-02-14 19:09:49
tags:
  - Linux
categories: [Operating system, Linux]
---

# Speed up apt installation or upgrade

## Overview

1. Get URIs of fetching files
2. Download them by aria2c to utilize maximum bandwidth
3. Install packages from the cache

## Script (apt-fast.sh)

```bash
#!/bin/bash

[ "`whoami`" = root ] || exec sudo bash "$0" "$@"

which aria2c &>/dev/null  || apt-get install -y --install-recommends aria2

if echo "$@" | grep -q "upgrade\|install\|dist-upgrade"; then
    cd /var/cache/apt/archives/

    deb_file_list=`mktemp`
    trap "rm -f ${deb_file_list}" EXIT
    apt-get -y --print-uris $@ | egrep -o -e "(http|https)://[^\']+" > ${deb_file_list}
    cat ${deb_file_list}

    aria2c -c -j 50 -x 5 -k 1m \
        --connect-timeout=15 --timeout=15 --max-tries=0 \
        --file-allocation=falloc --input-file=${deb_file_list}
    if [ "$?" = "0" ] ; then
        apt-get $@ -y
    else
        find /var/cache/apt/ -name "*.deb" -or -name "*.deb.aria2" | xargs -n 1 -r rm --
    fi
else
    apt-get $@
fi
```

## Usage

```bash
bash apt-fast.sh upgrade

bash apt-fast.sh dist-upgrade

bash apt-fast.sh install mssql-server mssql-server-fts mssql-tools

# https://docs.microsoft.com/en-us/sql/sql-server/

sudo /opt/mssql/bin/mssql-conf setup

systemctl status mssql-server --no-pager

echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc

sqlcmd -S localhost -U SA -P '<YourPassword>'

SELECT Name FROM sys.databases;
CREATE DATABASE TestDB;
USE TestDB;
GO
```
