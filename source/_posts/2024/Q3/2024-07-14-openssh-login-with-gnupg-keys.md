---
title: 'OpenSSH Login with GnuPG (OpenPGP) Keys'
excerpt: 'OpenSSH Login with GnuPG (OpenPGP) Keys'
date: 2024-07-14 12:17:52
tags:
  - Linux
  - Windows
  - Security
categories: [Utility, Security]
---

# OpenSSH Login with GnuPG Keys

Many people don't know that they can use **OpenPGP** (**GnuPG**) keys to log in to the **OpenSSH** server. Instead, they generate two sets of keys and use them independently. Using **OpenPGP** keys to log in to the **OpenSSH** server can make our lives more convenient and more secure. Let's start our journey.

## What You Need

- `GnuPG 2.1.9` or later

## Password Generator

Using secure passwords is one of the best security practices. It is recommended to use `apg` or `pwgen` as a password generator:

```bash
$ dpkg -l | grep -E 'apg|pwgen'
ii  apg                                        2.2.3.dfsg.1-5                        amd64        Automated Password Generator - Standalone version
ii  pwgen                                      2.08-2                                amd64        Automatic Password generation
```

Usage:

```bash
$ apg -a 1 -M SNCL -m 8 -x 8 -n 8
(8tY~>Cp
p4d$MYNH
'-xTtA5:
X3hS6vH&
~0r5uXwJ
v~r"6oQG
FF&*0f8(
;ykb0LZQ
```

```bash
$ pwgen -1 -s -B -y -n -c 8 8
X3h^Y/ve
e\E=w<7v
;b7AwVub
@\mC!3q;
4oA'`-Hv
#=otwp9F
N4:#P[=:
@Ftm=~3`
```

## Generate Key

Note that variable `PP` is the sample password, and `KF` is the generated master key fingerprint. Please replace them with your own corresponding password and master key fingerprint respectively.

```bash
PP='Vx@e0l6y'
gpg --quick-gen-key --batch --passphrase="${PP}" "X25519 User <x25519@example.com>" Ed25519 cert 1y
```

```bash
KF='EE84D73B6CEC596BDEB56EEF2F9DE5D06993DEF0'
gpg --quick-add-key --batch --passphrase="${PP}" --pinentry-mode loopback ${KF} ed25519 sign 1y
gpg --quick-add-key --batch --passphrase="${PP}" --pinentry-mode loopback ${KF} cv25519 encrypt 1y
gpg --quick-add-key --batch --passphrase="${PP}" --pinentry-mode loopback ${KF} ed25519 auth 1y
```

## List Key

Let's view the OpenPGP Keys we just generated:

```bash
$ gpg -K ${KF} --with-keygrip
sec   ed25519/2F9DE5D06993DEF0 2024-07-13 [C] [expires: 2025-07-13]
      Key fingerprint = EE84 D73B 6CEC 596B DEB5  6EEF 2F9D E5D0 6993 DEF0
uid                 [ultimate] X25519 User <x25519@example.com>
ssb   ed25519/A9E9E2021A84B4FB 2024-07-13 [S] [expires: 2025-07-13]
ssb   cv25519/140347FF8E0A799F 2024-07-13 [E] [expires: 2025-07-13]
ssb   ed25519/DCA34AA211A525C3 2024-07-13 [A] [expires: 2025-07-13]
```

Note that the purpose of the subkey `DCA34AA211A525C3` contains the letter `A` (`auth`), indicating that it can be used for authentication, such as logging into an OpenSSH server.

## Export SSH Key

Let's export the public key that the OpenSSH server needs:

```bash
$ gpg --export-ssh-key "${KF}"
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIMpjycTr1bb3jNBU/AdtsKjDX1Kz79pZsvz4kHnGM5wg openpgp:0x11A525C3
```

Note that the comment part `11A525C3` of this public key is the last 8 hexadecimal characters of the subkey `DCA34AA211A525C3`.

## Update ~/.ssh/authorized_keys in OpenSSH Server

The method of uploading the public key to the OpenSSH server depends on the configuration and deployment of the server. For common OpenSSH servers, it is usually directly added to the file `~/.ssh/authorized_keys` in the user directory:

```bash
echo 'ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIMpjycTr1bb3jNBU/AdtsKjDX1Kz79pZsvz4kHnGM5wg openpgp:0x11A525C3' >> ~/.ssh/authorized_keys
```

## Update ~/.gnupg/sshcontrol

In order to let the local `gpg-agent` know that this OpenPGP subkey can be used for OpenSSH authentication, we need to add its `keygrip` to the file `~/.gnupg/sshcontrol`.

Execute the following command to find the `keygrip` of this subkey:

```bash
gpg -K --with-keygrip ${KF}
sec   ed25519/2F9DE5D06993DEF0 2024-07-13 [C] [expires: 2025-07-13]
      Key fingerprint = EE84 D73B 6CEC 596B DEB5  6EEF 2F9D E5D0 6993 DEF0
      Keygrip = DFF57AE620812596C463FDC4110E15A56ED5ACE2
uid                 [ultimate] X25519 User <x25519@example.com>
ssb   ed25519/A9E9E2021A84B4FB 2024-07-13 [S] [expires: 2025-07-13]
      Keygrip = 1682922D586DDDFC461E73A7E5C91E7EF0BBA748
ssb   cv25519/140347FF8E0A799F 2024-07-13 [E] [expires: 2025-07-13]
      Keygrip = 038AA856070E9097CACB1939427393D1E0125E40
ssb   ed25519/DCA34AA211A525C3 2024-07-13 [A] [expires: 2025-07-13]
      Keygrip = 8C1FC82074A83ED40D851259694BBFB92B94A344
```

We can see that the `Keygrip` of subkey `DCA34AA211A525C3` is `8C1FC82074A83ED40D851259694BBFB92B94A344`, let's add it to the file `~/.gnupg/sshcontrol`:

```bash
$ echo '8C1FC82074A83ED40D851259694BBFB92B94A344' >> ~/.gnupg/sshcontrol
```

## Update ~/.gnupg/gpg-agent.conf

This step is optional and updates the file `~/.gnupg/gpg-agent.conf` to change the `TTL` of the key and to display the fingerprint of the SSH key using `SHA256` instead of `MD5`:

```
default-cache-ttl 1800
default-cache-ttl-ssh 1800
disable-scdaemon
enable-putty-support
enable-ssh-support
ssh-fingerprint-digest SHA256
```

## Update ~/.ssh/config

This step is also optional, because when `gpg-agent` is not started, **OpenSSH** cannot use public key authentication, so we use a feature of the OpenSSH client configuration to automatically execute the command `gpg-connect-agent`, which in turn uses a feature that automatically starts `gpg-agent` when it finds that it is not started:

```bash
$ vi ~/.ssh/config
# Linux
Match host * exec "gpg-connect-agent UPDATESTARTUPTTY /bye"

# Windows
Match host * exec "gpg-connect-agent UPDATESTARTUPTTY /bye >NUL 2>&1"
```

Note that although this step is optional, it executes `UPDATESTARTUPTTY`, which ensures that `gpg-agent` pops up the **password input box** in the current window and does not find the wrong window. This is particularly useful on **Linux** systems, especially when using terminal multiplexers such as **tmux**.

## Update ~/.bashrc

For example, in the file `~/.bashrc`, `~/.bash_profile` or `~/.profile`, in the interactive login section, configure the environment variable `GPG_TTY`:

```bash
export GPG_TTY=`tty`
# gpg-connect-agent UPDATESTARTUPTTY /bye
# ssh-add -l
```

If you do not want to log in again, you can execute the following command to make it take effect:

```bash
$ source ~/.bashrc
```

or:

```bash
$ export GPG_TTY=`tty`
```

## Try OpenSSH Login

Now that everything is done, let's try logging into OpenSSH using our GnuPG key:

```bash
$ ssh <user>@example.com

┌──────────────────────────────────────────────────────┐
│ Please enter the passphrase for the ssh key          │
│ SHA256:ZbVC8wNc1vdBSNDh7+xfupzc0bDUEElJZ3EyGxuzUVU   │
│                                                      │
│                                                      │
│ Passphrase: ________________________________________ │
│                                                      │
│       <OK>                              <Cancel>     │
└──────────────────────────────────────────────────────┘
```

Enter the password we provided when generating the **OpenPGP** key, and we are logged in to the remote **OpenSSH** server.

## Mapping of OpenPGP Keys and SSH Fingerprints

Sometimes we have many keys available for SSH login, and we may not be able to find the **OpenPGP key** corresponding to the **SSH fingerprint**. In this case, the following script is very useful:

```bash
#!/bin/bash

gpg_keys=$(gpg -K --list-options show-usage | grep -E '(^sec|^ssb)' | awk '$4 ~ /A/ {print $2}')

for key in $gpg_keys; do
    fingerprint=$(echo $key | cut -d'/' -f2)

    gpg_ssh_fingerprint=$(gpg --export-ssh-key $fingerprint! | ssh-keygen -lf - | awk '{print $2}')
    ssh_keys=$(ssh-add -l | awk '{print $2}')

    for ssh_key in $ssh_keys; do
        if [ "$gpg_ssh_fingerprint" == "$ssh_key" ]; then
            echo "OpenPGP $key -> SSH $ssh_key"
        fi
    done
done


OpenPGP rsa2048/0401AA2046D397FF -> SSH SHA256:Oa8egR/Az0cyo44IjvkrzHKownGgo59LctsMmEBNXyo
OpenPGP ed25519/CB7D373D4F999240 -> SSH SHA256:ZbVC8wNc1vdBSNDh7+xfupzc0bDUEElJZ3EyGxuzUVU
...
```

## Delete Key

Now that the test is complete, let's delete the OpenPGP test key we just generated:

```bash
$ gpg --delete-key --batch --yes ${KF}
```

In addition, don't forget to delete the records we added in the file `~/.gnupg/sshcontrol` and the file `~/.ssh/authorized_keys`. The changes to other files can be retained. Of course, if you want to delete **GnuPG** as well, you also need to delete the records we added in the file `~/.ssh/config`.

## Wrapping Up

The pursuit of **security**, **reliability** and **simplicity** has always been one of our goals. Let's start by using **OpenPGP** keys and **GnuPG** to log in to the **OpenSSH** server!

## Reference

- https://www.gnupg.org/download/
- https://www.gnupg.org/documentation/manuals/gnupg/
- https://www.gnupg.org/documentation/manuals/gnupg/OpenPGP-Key-Management.html
- https://www.gnupg.org/documentation/manuals/gnupg/Operational-GPG-Commands.html
- [TOFU not affected by Key deletion - https://dev.gnupg.org/T2859](https://dev.gnupg.org/T2859)
- https://github.com/PowerShell/Win32-OpenSSH/releases/
- https://cheatsheets.chaospixel.com/gnupg/
