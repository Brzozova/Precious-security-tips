## Nmap

```
nmap -sC -sV <IP>
```

-sC – to scan using the default nmap scripts
-sV – to pull version information of open ports found during the scan

```
nmap -sV -p- -vv <IP>
```


## Password cracker

### Hydra

```
hydra -l <username> -P <path_to_wordlist> <IP> ftp
```


### Stegcracker

```
stegcracker file.jpg <path_to_wordlist>
```



## Find hidden webserver endpoints

### Gobuster

```
gobuster dir -u http://<IP> -w <path_to_wordlist>
```

dir – to use directory/file brute-forcing mode
-u – is the flag to tell gobuster that we are scanning a URL
-w – is the flag to set the list of possible directory and file names








