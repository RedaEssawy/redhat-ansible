Note:
/etc file has the configuration files of the system.

# `tar` command options
- `-c,--create`: Create a new achive.
- `-x,--extract`: Extract from an exiting archive.
- `-t,--list`: List the table of contents of an archive
- `-v, --verbose`: Verbose. Show which files get archived or extracted.
- `-f, --file=`: File name. This option must be followed by the file name of the archive to use or create.
- `-p, --preserve-permissions`: Preserve the permissions of the files and directories when extracting an archive, without subtracting the umask.

Note: Archiving process dosn't compress the files.

Create an archive from /etc
```bash
tar -cvf etc-bachup-$(date +%F).tar /etc
```
```bash
# to show the size of each file on /etc
du /etc

# to show the summary size of /etc
du -s /etc

# with `h` option for human rediable
du -sh /etc

```
Extract the archived file
```bash
tar -xf etc-backup-*
```
Then 
```bash
cd etc
ls
``

# Creating a compressed archive
## the `tar` command supports three compression methods:
- gzip
- bzip2
- xz

# Use one of the following options to create a compressed `tar` archive:
- `-z, --gzip`: Use `gzip` compression (.tar.gz).
- `-j, --bzip2`: Use `bzib2` compression (.tar.bz2). `bzip2` typically achieves a better compression ratio than `gzip`.
- `-J, --xz`: Use `xz` compression (.tar.xz). The `xz` compression typically achieves a better compression ratio than `bzip2`.

Archive and compress using gzip.
```bash
tar -czf ect_backup.tar.gz etc
```
Note the size:
```bash
du -sh etc_backup.tar.gz
```
Archive and compress using bzip2.
```bash
tar -cjf ect_backup.tar.bz2 etc
```
```
du -sh ect_backup.tar.bz2
```
Archive and compress using xz.
```bash
tar -cJf etc_bachup.tar.xz etc
```

```
du -sh ect_bachup.tar.xz
```
To extract the compressed files. the same commands with replacement c to x

```bash
# if gzip
tar -xzf etc_backup.tar.gz

# if bzip2
tar -xjf etc_backup.tar.bz2

# if xz
time tar -xJf etc_backup.tar.xz
```
`time` option to calculate the time of execution of the command.

# You can use gzip, bzip2z, and xz to compress the archived files and uncompress them using gunzip, bunzip2, and unxz.
Note you can't compress files without archive them you should to archive them first.

# To transfer files from or to remote machines you have two options:
## First option

```bash
# from your to remote
scp file1 file2 file3 remoteuser@remoteip:/path/to/distination

# from remote to local
scp remoteuser@remoteip:/path/to/source  distination

if transfer a directory you should use `-r` option
```
## Second option
### Interactive session (shell)

Option session with remote machine using `sftp` protocol.

```bash
sftp remoteuser@remoteip
```

Run a command on the remote.

```bash
pwd

# the file1 will created on remote server.
mkdir file1
```
To run a command on local add `l` before the command.

```bash
lpwd

lls

```
To transfer a file to remote server use `put` command.

```bash
put localfile
```

To transfer a file to local server use `get` command.

```bash
get remotefile
```

To terminate the session run `exit` command.
