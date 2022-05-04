# Mail protocols

## Components & process

Email delivery over the Internet requires:
* Mail Submission Agent (MSA)
* Mail Transfer Agent (MTA)
* Mail Delivery Agent (MDA)
* Mail User Agent (MUA)

### Process
1. A Mail User Agent (MUA), or simply an email client, has an email message to be sent. The MUA connects to a Mail Submission Agent (MSA) to send its message.
2. The MSA receives the message, checks for any errors before transferring it to the Mail Transfer Agent (MTA) server, commonly hosted on the same server.
3. The MTA will send the email message to the MTA of the recipient. The MTA can also function as a Mail Submission Agent (MSA).
4. A typical setup would have the MTA server also functioning as a Mail Delivery Agent (MDA).
5. The recipient will collect its email from the MDA using their email client.

## Protocols

### Simple Mail Transfer Protocol (SMTP)

Port: 25
Data security: Cleartext

Used to communicate with an MTA server. Because SMTP uses cleartext, where all commands are sent without encryption, 
we can use a basic Telnet client to connect to an SMTP server and act as an email client (MUA) sending a message.

Connecting 
```
$ telnet <IP> 25
```

### Post Office Protocol version 3 (POP3)

Port: 110
Data security: Cleartext

Used to download the email messages from a Mail Delivery Agent (MDA) server.
Mail client (MUA) will connect to the POP3 server (MDA), authenticate, and download the messages. 
The communication using the POP3 protocol will be hidden behind a sleek interface, similar commands will be issued, as in the Telnet session.

Authentication is required to access the email messages; the user authenticates by providing his username 
USER <username> - providing username
PASS <password> - providing password
* STAT - reply eg. +OK 1 179; based on RFC 1939, a positive response to STAT has the format +OK nn mm
  nn is the number of email messages in the inbox,
  mm is the size of the inbox in octets (byte). 
* LIST - provided a list of new messages on the server
* RETR 1 retrieved the first message in the list.

### Internet Message Access Protocol (IMAP) 
  
Port: 143
Data security: Cleartext

Makes it possible to keep your email synchronized across multiple devices (and mail clients). 
In other words, if you mark an email message as read when checking your email on your smartphone, 
the change will be saved on the IMAP server (MDA) and replicated on your laptop when you synchronize your inbox.

  
  
