---
title: 'Console Based Gmail Client'
excerpt: 'Console Based Gmail Client'
date: 2023-02-04 15:39:15
tags:
  - Linux
  - Windows
categories:
  - Operating system
---

# Console Based Gmail Client

+ https://billauer.co.il/blog/2022/10/git-send-email-with-oauth2-gmail/
+ https://mmogilvi.users.sourceforge.net/software/oauthbearer.html
+ https://wiki.archlinux.org/title/mutt
+ https://wiki.archlinux.org/title/OfflineIMAP
+ http://www.mutt.org/doc/manual/

## Incoming Mail (IMAP) Server

```
imap.gmail.com
Requires SSL: Yes
Port: 993
```

## Outgoing Mail (SMTP) Server

```
smtp.gmail.com
Requires SSL: Yes
Port for SSL: 465
Requires TLS: Yes
Port for TLS/STARTTLS: 587
Requires Authentication: Yes
```

```
openssl s_client -connect smtp.gmail.com:465 -crlf
openssl s_client -connect smtp.gmail.com:587 -crlf -starttls smtp
```

```
# msmtp -S --host=smtp.gmail.com --port=465 --tls=on --tls-starttls=off
# msmtp -S --host=smtp.gmail.com --port=587 --tls=on --tls-starttls=on
SMTP server at smtp.gmail.com (bi-in-f109.1e100.net [2607:f8b0:4004:c08::6d]), port 587:
    smtp.gmail.com ESMTP 16-20020a05620a041000b007201df7ff47sm11422373qkp.45 - gsmtp
TLS session parameters:
    (TLS1.3)-(ECDHE-X25519)-(ECDSA-SECP256R1-SHA256)-(AES-256-GCM)
TLS certificate information:
    Subject:
        CN=smtp.gmail.com
    Issuer:
        C=US,O=Google Trust Services LLC,CN=GTS CA 1C3
    Validity:
        Activation time: Mon 09 Jan 2023 04:18:39 PM CST
        Expiration time: Mon 03 Apr 2023 04:18:38 PM CST
    Fingerprints:
        SHA256: 46:22:44:D3:E5:6D:94:A5:CE:18:AE:DB:E2:30:05:82:B5:18:0D:10:3C:B3:53:B2:DC:3D:AD:1F:A2:1A:43:97
        SHA1 (deprecated): 23:D7:F1:24:51:BE:47:68:6D:2A:2C:28:37:A8:8E:1A:53:41:F9:70
Capabilities:
    SIZE 35882577:
        Maximum message size is 35882577 bytes = 34.22 MiB
    PIPELINING:
        Support for command grouping for faster transmission
    STARTTLS:
        Support for TLS encryption via the STARTTLS command
    AUTH:
        Supported authentication methods:
        PLAIN LOGIN OAUTHBEARER XOAUTH2
```

## Mail Transfer Agent (MTA)

### dma

```
vi /etc/dma/auth.conf
    me@gmail.com|smtp.gmail.com:PaSsWorD
```

```
vi /etc/dma/dma.conf
    AUTHPATH /etc/dma/auth.conf
    SECURETRANSFER
    SMARTHOST smtp.gmail.com
    PORT 465
```

```
vi /etc/dma/dma.conf
    AUTHPATH /etc/dma/auth.conf
    SECURETRANSFER
    SMARTHOST smtp.gmail.com
    PORT 587
    STARTTLS
```

```
cat << EOF | dma -D it@gmail.com
From: me <me@gmail.com>
To: it <it@gmail.com>
Subject: title
Content-Type: text/plain; charset="UTF-8"

Hi,

Sent from dma #1.

Thanks,
me
EOF
```

### msmtp

```
vi ~/.msmtprc

account default
auth on
tls on
logfile ~/.msmtp.log

host smtp.gmail.com
from me@gmail.com

user me@gmail.com
password PaSsWorD
# gpg -vv --encrypt -o ~/.msmtp-password.gpg -r me@gmail.com -
# gpg -c -vv --cipher-algo AES256 -o ~/.msmtp-password.gpg -
# passwordeval gpg --no-tty -q -d ~/.msmtp-password.gpg

# port 465
# tls_starttls off

port 587
tls_starttls on
```

```
cat << EOF | msmtp -v it@gmail.com
From: me <me@gmail.com>
To: me <it@gmail.com>
Subject: title
Content-Type: text/plain; charset="UTF-8"

Hi,

Sent from msmtp #1.

Thanks,
me
EOF
```

### ssmtp

```
vi /etc/ssmtp/revaliases

# Format:       local_account:outgoing_address:mailhub
root:me@gmail.com:smtp.gmail.com:465
```

```
vi /etc/ssmtp/ssmtp.conf
chmod 0666 /etc/ssmtp/ssmtp.conf

FromLineOverride yes
Mailhub smtp.gmail.com:465
UseTLS=YES
#Mailhub smtp.gmail.com:587
#UseSTARTTLS=YES
AuthMethod=LOGIN
AuthUser=me@gmail.com
AuthPass=PaSsWorD
```

```
cat << EOF | ssmtp -v it@gmail.com
From: me <me@gmail.com>
To: me <it@gmail.com>
Subject: title
Content-Type: text/plain; charset="UTF-8"

Hi,

Sent from ssmtp #1.

Thanks,
me
EOF
```

## Mail Delivery Agent (MDA)
+ [2023-01-28, fetchmail 6.4.36](https://www.fetchmail.info/)
+ [2023-01-21, getmail 6.18.12](https://github.com/getmail6/getmail6)

### fetchmail

```
PATH=/usr/bin:/usr/ucb:/bin:/usr/local/bin:.
MAILDIR=$HOME/Mail      # You'd better make sure it exists
DEFAULT=$MAILDIR/mbox
LOGFILE=$MAILDIR/log
LOCKFILE=$HOME/.lock
```

```
touch ~/.fetchmailrc && chmod 600 $_

vi ~/.fetchmail.rc

poll imap.gmail.com service 993 protocol IMAP
    username "me@gmail.com"
    password "PaSsWorD"
    options ssl sslproto TLS1.2+
    mimedecode keep fetchlimit 100
    mda "/usr/bin/procmail -f %F -d %T"


/etc/default/fetchmail
/etc/init.d/fetchmail
/etc/logcheck/ignore.d.server/fetchmail
/etc/logcheck/ignore.d.workstation/fetchmail
/etc/ppp/ip-down.d/fetchmail
/etc/ppp/ip-up.d/fetchmail
/etc/resolvconf/update-libc.d/fetchmail
/usr/lib/systemd/user/fetchmail.service
/usr/share/doc/fetchmail/examples/fetchmailrc.example
```

```
fetchmail -f ~/.fetchmail.rc --verbose
```

## Mail User Agent (MUA)

### mutt

```
vi ~/.muttrc

set my_user = "me@gmail.com"
# gpg -vv --encrypt -o ~/.mutt-password.gpg -r me@gmail.com -
# gpg -c -vv --cipher-algo AES256 -o ~/.mutt-password.gpg -
# set my_pass = "PaSsWorD"
source "gpg -dq $HOME/.mutt-password.gpg |"

set realname = "me"
# set signature="path/to/sig/file" # ~/.signature
set from = $my_user
set use_from = yes
set send_charset="utf-8"
set sort = threads

set mbox_type=Maildir
set date_format="%y-%m-%d %T"
set index_format="%2C | %Z [%d] %-30.30F (%-4.4c) %s"

# type ‘c’ to change to another folder
set folder = imaps://imap.gmail.com/
set spoolfile = +INBOX
set postponed = +[Gmail]/Drafts
set trash = "+[Gmail]/Trash"
# Gmail automatically saves sent e-mail to +[Gmail]/Sent, so we do not want duplicates.
# set record = +[Gmail]/Sent
unset record
set imap_user=$my_user
set imap_pass=$my_pass
set imap_check_subscribed
set imap_keepalive = 300
unset imap_passive
set mail_check = 60

set smtp_url=smtps://$my_user:$my_pass@smtp.gmail.com

# set sendmail="/usr/bin/msmtp"
# set sendmail="/usr/sbin/dma"
# set sendmail="/usr/sbin/ssmtp"
```
