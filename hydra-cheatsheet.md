# Hydra

Supports: eg. FTP, POP3, IMAP, SMTP, SSH, and all methods related to HTTP
Syntax: `hydra -l username -P wordlist.txt server service`

## Useful flags

-s PORT - to specify a non-default port for the service in question.
-V or -vV - makes Hydra show the username and password combinations that are being tried. 
            This verbosity is very convenient to see the progress, especially if you are still not confident of your command-line syntax.
-t n - where n is the number of parallel connections to the target. -t 16 will create 16 threads used to connect to the target.
-d - debugging, to get more detailed information about what’s going on. The debugging output can save you much frustration; for instance, if Hydra tries to connect to a closed port and timing out, -d will reveal this right away.
