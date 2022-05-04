# Secure protocols

* Encryption for protocols is added in Presentation Layer (6).

## How to upgrade protocols to use encryption via SSL/TLS?

Protocols after the upgrade
HTTP	80 ->	HTTPS	443
FTP	21 ->	FTPS	990
SMTP	25 ->	SMTPS	465
POP3	110 ->	POP3S	995
IMAP	143	-> IMAPS	993
DNS 53 -> DoT (DNS over TLS) 853

## How HTTP vs HTTPS works?

HTTP process:
1. Establish a TCP connection with the remote web server
2. Send HTTP requests to the web server, such as GET and POST requests.

HTTPS process:
1. Establish a TCP connection
2. Establish SSL/TLS connection
    SSL/TLS handshake:
    1. The client sends a ClientHello to the server to indicate its capabilities, such as supported algorithms.
    2. The server responds with a ServerHello, indicating the selected connection parameters. The server provides its certificate if server authentication is required. The certificate is a digital file to identify itself; it is usually digitally signed by a third party. Moreover, it might send additional information necessary to generate the master key, in its ServerKeyExchange message, before sending the ServerHelloDone message to indicate that it is done with the negotiation.
    3. The client responds with a ClientKeyExchange, which contains additional information required to generate the master key. Furthermore, it switches to use encryption and informs the server using the ChangeCipherSpec message.
    4. The server switches to use encryption as well and informs the client in the ChangeCipherSpec message.
3. Send HTTP requests to the webserver


