## Samba

## Tips
* There are SMB share drives on a server that can be connected to and used to view or transfer files. 
* Default ports are 445 & 139


## Enumerate SMB shares

Tool: Enum4linux -> https://github.com/CiscoCXSecurity/enum4linux

Example command:
```
$ enum4linux -a <IP>
```

Enum4linux flags:
-U - get userlist
-M - get machine list
-N - get namelist dump (different from -U and-M)
-S - get sharelist
-P - get password policy information
-G - get group and member list
-a - all of the above (full basic enumeration)


## Remote access to SMB share

Tool: SMBclient

Example:
```
$ smbclient //<IP>/<share_name> -U <username> -P <port>
```
Flags:
-U - specify the user
-p - specify the port


