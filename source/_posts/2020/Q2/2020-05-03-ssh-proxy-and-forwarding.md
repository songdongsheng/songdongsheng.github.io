---
title: SSH Proxy and Forwarding
description: SSH Proxy and Forwarding
date: 2020-05-03 15:38:17
tags:
  - Operating system
  - Linux
categories: [Operating system, Linux]
permalink: ssh-proxy-and-forwarding
---

# SSH Proxy and Forwarding

SSH not only can be used as a SOCKS5 proxy to access the remote network, but also be used to do port forwarding from local to the given remote side, or vice versa.

## SOCKS5 proxy to access the remote network

```shell
while true; do
    date
    ssh -C -N -T -D 4080 user@example.com
    echo
done
```

## SOCKS5 proxy to access the local network

```shell
while true; do
    date
    ssh -C -N -T -R 4080 user@example.com
    echo
done
```

## Forward local traffic to the remote side

```shell
while true; do
    date
    ssh -C -N -T -L 127.0.0.1:4080:192.168.121.121:80 user@example.com
    echo
done
```

## Forward remote traffic to the local side

```shell
while true; do
    date
    ssh -C -N -T -R 127.0.0.1:4080:10.20.30.40:80 user@example.com
    echo
done
```

## Cookbook

### options

```
     -4      Forces ssh to use IPv4 addresses only.
     -6      Forces ssh to use IPv6 addresses only.

     -C      Requests compression of all data (including stdin, stdout, stderr, and data for forwarded X11, TCP and
             UNIX-domain connections).  The compression algorithm is the same used by gzip(1).  Compression is desirable on
             modem lines and other slow connections, but will only slow down things on fast networks.  The default value can
             be set on a host-by-host basis in the configuration files; see the Compression option.

     -N      Do not execute a remote command.  This is useful for just forwarding ports.

     -L [bind_address:]port:host:hostport
     -L [bind_address:]port:remote_socket
     -L local_socket:host:hostport
     -L local_socket:remote_socket
             Specifies that connections to the given TCP port or Unix socket on the local (client) host are to be forwarded
             to the given host and port, or Unix socket, on the remote side.  This works by allocating a socket to listen to
             either a TCP port on the local side, optionally bound to the specified bind_address, or to a Unix socket.  When‐
             ever a connection is made to the local port or socket, the connection is forwarded over the secure channel, and
             a connection is made to either host port hostport, or the Unix socket remote_socket, from the remote machine.

             Port forwardings can also be specified in the configuration file.  Only the superuser can forward privileged
             ports.  IPv6 addresses can be specified by enclosing the address in square brackets.

             By default, the local port is bound in accordance with the GatewayPorts setting.  However, an explicit
             bind_address may be used to bind the connection to a specific address.  The bind_address of “localhost” indi‐
             cates that the listening port be bound for local use only, while an empty address or ‘*’ indicates that the port
             should be available from all interfaces.

     -R [bind_address:]port:host:hostport
     -R [bind_address:]port:local_socket
     -R remote_socket:host:hostport
     -R remote_socket:local_socket
     -R [bind_address:]port
             Specifies that connections to the given TCP port or Unix socket on the remote (server) host are to be forwarded
             to the local side.

             This works by allocating a socket to listen to either a TCP port or to a Unix socket on the remote side.  When‐
             ever a connection is made to this port or Unix socket, the connection is forwarded over the secure channel, and
             a connection is made from the local machine to either an explicit destination specified by host port hostport,
             or local_socket, or, if no explicit destination was specified, ssh will act as a SOCKS 4/5 proxy and forward
             connections to the destinations requested by the remote SOCKS client.

             Port forwardings can also be specified in the configuration file.  Privileged ports can be forwarded only when
             logging in as root on the remote machine.  IPv6 addresses can be specified by enclosing the address in square
             brackets.

             By default, TCP listening sockets on the server will be bound to the loopback interface only.  This may be over‐
             ridden by specifying a bind_address.  An empty bind_address, or the address ‘*’, indicates that the remote
             socket should listen on all interfaces.  Specifying a remote bind_address will only succeed if the server's
             GatewayPorts option is enabled (see sshd_config(5)).

             If the port argument is ‘0’, the listen port will be dynamically allocated on the server and reported to the
             client at run time.  When used together with -O forward the allocated port will be printed to the standard out‐
             put.

     -X      Enables X11 forwarding.  This can also be specified on a per-host basis in a configuration file.

             X11 forwarding should be enabled with caution.  Users with the ability to bypass file permissions on the remote
             host (for the user's X authorization database) can access the local X11 display through the forwarded connec‐
             tion.  An attacker may then be able to perform activities such as keystroke monitoring.

             For this reason, X11 forwarding is subjected to X11 SECURITY extension restrictions by default.  Please refer to
             the ssh -Y option and the ForwardX11Trusted directive in ssh_config(5) for more information.

             (Debian-specific: X11 forwarding is not subjected to X11 SECURITY extension restrictions by default, because too
             many programs currently crash in this mode.  Set the ForwardX11Trusted option to “no” to restore the upstream
             behaviour.  This may change in future depending on client-side improvements.)

     -f      Requests ssh to go to background just before command execution.  This is useful if ssh is going to ask for pass‐
             words or passphrases, but the user wants it in the background.  This implies -n.  The recommended way to start
             X11 programs at a remote site is with something like ssh -f host xterm.

             If the ExitOnForwardFailure configuration option is set to “yes”, then a client started with -f will wait for
             all remote port forwards to be successfully established before placing itself in the background.

     -n      Redirects stdin from /dev/null (actually, prevents reading from stdin).  This must be used when ssh is run in
             the background.  A common trick is to use this to run X11 programs on a remote machine.  For example, ssh -n
             shadows.cs.hut.fi emacs & will start an emacs on shadows.cs.hut.fi, and the X11 connection will be automatically
             forwarded over an encrypted channel.  The ssh program will be put in the background.  (This does not work if ssh
             needs to ask for a password or passphrase; see also the -f option.)
```

### IPv4

Running a local SOCKS5 proxy on TCP port 4080, and forward connection is made from the remote machine on the TCP port 4022, to the local host TCP port 22.

```shell
while true; do
    date
    ssh -C -N -T -4 -D 4080 -R 127.0.0.1:4022:127.0.0.1:22 user@example.com
    echo
done
```

### IPv6

Running a local SOCKS5 proxy on TCP port 6080, and forward connection is made from the remote machine on the TCP port 6022, to the local host TCP port 22.

```shell
while true; do
    date
    ssh -C -N -T -6 -D 6080 -R [::1]:6022:[::1]:22 user@example.com
    echo
done
```
