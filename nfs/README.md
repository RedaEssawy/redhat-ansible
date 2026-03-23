# NFS 
## The steps to chare file between linux machines

## Step1
### install nfs-utils packages on all machines
```bash
# If redhat
sudo dnf install nfs-utils

# If Ubuntu

sudo apt install nfs-utils
```



## On Server machine

### Configure the firewall
You hav two options:
1. stop temporary the firewall
```bash
systemctl stop firewalld
```
2. Enable NFS on the firewall
```bash
firewall-cmd --permanent --add-service=nfs
```

### Write the files that you need to share on /etc/exports
```bash
vim /etc/exports
```
### After this
```bash
# To show the shared files
exportfs 
# To apply all changes on shared file (/etc/exports)
exportfs -a
# Or to apply 
exportfs -r
exportfs -f

# To unexport for all files
exportfs -ua

# To show the all default roles
exportfs -v

# To get more details about options of exports
# 5: is the section that has the info about exports
man 5 exports

# Or restart the nfs server but NOT RECOMMENDED
```

## On the Client machines
### You need to only mount the shared files

### To show files that shared with this machine
```bash
showmount -e serverip
```
NOTES:
You may be encounter an error message:
<per>
clnt_create: RPC: Unable to receive
</pre>
This issues needs to enable rpc-bin, mountd on firewall on the server machine

```bash
firewall-cmd --permanent --add-service=rpc-bind

firewall-cmd --permanent --add-service=rpc-bind

firewall-cmd --reload
```


You hav three options:
1. Manual method (temporary) if reboot the system it will be deleted
```bash
# to show the shared files
showmount -e serverip

# Mount the file
mount -t nfs serverip:/ /mnt

# to show files
df -h

cd /mnt
# to list the shared files 
ls 

# other metho 
mkdir /mnt/external

mount -t nfs serverip:/external /mnt/external

df -hT

```

2. permanent method
# Create a Directory to mount the shared files on it
```bash
mkdir /mnt/part3

vim /etc/fstab
# Add this line
192.168.1.10:/mdia/reda/part3 /mnt/part3   nfs      defaults 0 0

# Save and exit
# mount 
mount -a

# Any change on fstab file run this command to make the system aware about this changes
systemctl daemon-reload

# mount again
mount -a
```


3. On demand method
## Steps:
1. Install autofs package:
```bash
sudo dnf install autofs
```
2. Tow steps:
-  Add master map file under /etc/auto.master.d
```bash
# the file name is demo.autofs the .autofs extension is mendatory.

vim /etc/auto.master.d/demo.autofs
```
- Add the master map entry in the previous created file, in this case, for indirectly mapped mounts:
```bash
# /share: base directory
# /etc/auto.demo: the map file that you will put on it all  your mount datials(mount point, mount options, or source location) (auto.demo: any name for your file) 
/share /etc/auto.demo
```
3. Create the mapping files.
```bash
vim /etc/auto.demo
```
The mapping file-naming convention is /etc/auto.name. where name reflects the content of the map
```bash
work -rw,sync serverb:/shares/work
```
4. Start and enable the automounter service.
```bash
sudo systemctl enable --now autofs
```
# Direct maps:
## To use directly mapped mount points, the master map file shold appears as follows:
```bash
/- /etc/auto.direct

# All direct map entries use /- as the base directory in this case. the mapping file that contains the mount details is `/etc/auto.direct`.

```
## The content for the /etc/auto.direct file might appears as follows:
```bash
/mnt/docs -rw,sync serverb:/shares/docs
```
 The mount point (or key) is always an absolute path. the rest of the mapping file uses the same structure.
In this example the /mnt directory exists, and it is not managed by autofs. the full directory /mnt/docs will be created and removed automatically by the autofs service.

# Indirect Willcard Maps:
## Is more dynamic.
## The content for the /etc/auto.demo file might appears as follows:
```bash
* -rw,sync serverb:/shares/&
```
The mout point (or key) > *
The source location is > &
Everything is the same.



![Automount benefits](screens/automount.png)
![Steps](screens/steps-of-automount.png)
![Steps](screens/steps-of-automount2.png)
![Direct map](screens/directmap.png)
![Indirect map](screens/indirectmap.png)


