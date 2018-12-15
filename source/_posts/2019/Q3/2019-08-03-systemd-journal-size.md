---
title: Systemd's Journal Size
description: Systemd's Journal Size
date: 2019-08-03 23:57:19
tags:
  - Linux
categories: [Operating system, Linux]
permalink: systemd-journal-size
---

# Systemd's Journal Size

## journald.conf

```bash
$ grep -v "#" /etc/systemd/journald.conf
[Journal]
Compress=yes
SystemMaxUse=512M
SystemMaxFileSize=64M
RuntimeMaxUse=2048M
RuntimeMaxFileSize=64M

$ systemctl restart systemd-journald
```

## Show total disk usage of all journal files

```bash
$ du -sh /var/log/journal
529M    /var/log/journal

$ journalctl --disk-usage
Archived and active journals take up 528.0M on disk.
```

## Reduce disk usage manually

```bash
# journalctl --vacuum-files=63
# journalctl --vacuum-time=7d
# journalctl --vacuum-size=512M

Vacuuming done, freed 104.0M of archived journals on disk.
journal verify
```

## Verify journal file consistency

```bash
# journalctl --verify
```
