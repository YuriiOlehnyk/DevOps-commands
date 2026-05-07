# Linux Commands Cheatsheet

---

### Navigation

```bash
pwd                  # print current working directory
ls -la               # list all files with details
ls -Rla Dir1         # list hierarchy in Dir1 recursively
cd ~                 # go to home directory
cd -                 # go to previous directory
tree                 # visual directory tree
pushd / popd         # save & return to directory
```

> **Flags**
> `-l` long format · `-a` show hidden files · `-R` recursive · `-h` human readable sizes

---

### Files & Directories

```bash
mkdir -p a/b/c                  # create nested directories
cp -r src/ dst/                 # copy directory recursively
mv old new                      # move or rename file/directory
rm -rf dir/                     # delete directory and contents (careful!)
ln -s target link               # create symbolic link
ln file1 file1duplicate         # create hard link (duplicate)
touch file.txt                  # create empty file
touch -t YYYYMMDDhhmm filename  # create file with specific timestamp
cp file-$(date +%s).conf.bak    # backup file with epoch timestamp
```

> **Flags**
> `-p` create parent dirs · `-r` recursive · `-f` force · `-s` symbolic link
> `+%s` Unix epoch timestamp (seconds since 1970-01-01)

---

### View & Edit

```bash
cat file          # print file contents
less file         # scroll through file (q to quit)
more file         # scroll through file
head -n 20 file   # show first 20 lines
tail -f file      # follow live output
nano / vim file   # edit file in terminal
wc -l file        # count lines in file
```

> **Flags**
> `-n` number of lines · `-f` follow file live updates

---

### Search & Find

```bash
grep -r 'text' .        # search text recursively in current dir
grep -i 'text' file     # case-insensitive search
find . -name '*.log'    # find files by name pattern
find . -mtime -1        # find files modified less than 1 day ago
locate filename         # fast system-wide file search
which cmd               # show full path of a command
```

> **Flags**
> `-r` recursive · `-i` case-insensitive · `-name` filter by filename · `-mtime` filter by modification time

---

### Permissions

```bash
chown username:groupname file       # change file owner and group
chgrp groupname file                # change file group only
chmod a+x file                      # add execute for all (u/g/o)
chmod g-rw file                     # remove read/write from group
chmod o=rw file                     # set read/write for others exactly
chmod 777 file                      # set rwx for everyone
chmod 741 file                      # rwx for owner, r for group, x for others
chmod 1777 dir                      # enable sticky bit
chmod 0777 dir                      # disable sticky bit
sudo chattr +i filename             # make file immutable (undeletable by anyone)/ Sticky bit
sudo chattr -i filename             # remove immutable attribute
lsattr filename                     # check file attributes
sudo find / -user OLD_UID -exec chown -h NEW_UID {} \;   # change ownership of all files by UID
sudo find / -group OLD_GID -exec chgrp -h NEW_GID {} \;  # change group of all files by GID
```

> **Flags**
> `u` user · `g` group · `o` other · `a` all (ugo)
> `r=4` · `w=2` · `x=1` — combine for octal: rwx=7, rw=6, rx=5
> `+i` immutable · `-exec` execute command per found file · `-h` affect symlinks

---

### User Management

```bash
useradd -m username                    # create user with home directory
userdel -r username                    # delete user and home directory
sudo useradd -u 1500 username          # create new user with specific UID
sudo usermod -u NEW_UID username       # change existing user UID
sudo usermod -aG groupname username    # add user to supplementary group
sudo usermod -g groupname username     # change user primary group
sudo usermod -m -d /new/home/path username  # set new home directory
sudo usermod -aG sudo username         # grant sudo rights to user
sudo passwd username                   # set or change user password
id username                            # verify user UID and groups
```

> **Flags**
> `-m` create/move home directory · `-u` set UID · `-aG` append supplementary group
> `-g` set primary group · `-d` set home directory path · `-r` remove home on delete

---

### Group Management

```bash
groupadd Programmers                   # create new group
groupdel Programmers                   # delete a group
sudo groupadd -g 1500 groupname        # create group with specific GID
sudo groupmod -g NEW_GID groupname     # change existing group GID
sudo groupmod -n new_name old_name     # rename a group
usermod -aG Programmers username       # add user to group
deluser username Programmers           # remove user from group
```

> **Flags**
> `-g` set GID · `-n` rename group · `-aG` append to group

---

### Sudo Configuration

```bash
sudo visudo -f /etc/sudoers.d/username   # create sudoers rule file (safe, no default file edit)
sudo -l -U username                      # list sudo privileges for a user
```

> Rule syntax inside sudoers file:
```
student ALL=(ALL) ALL
student ALL=(ALL) NOPASSWD: /usr/bin/apt install, /usr/bin/apt-get install
```

> **Options**
> `ALL=(ALL)` on all hosts, as any user · `NOPASSWD:` no password for listed commands

---

### Processes

```bash
ps aux              # list all running processes
top                 # live process monitor (Shift+P sort by CPU, Shift+M by memory)
htop                # interactive live process monitor
kill -9 PID         # force kill process by PID
pkill name          # kill process by name
jobs / bg / fg      # manage background jobs
nice -n 10 cmd      # run command with lower priority
```

> **Flags**
> `-9` force kill signal · `-n` niceness value (higher = lower priority)

---

### Networking

```bash
hostname -I                                  # show local IP address
curl ifconfig.me                             # show external/public IP address
ping host                                    # check connectivity to host
ss -tulnp                                    # show open ports and sockets
ssh user@host                                # connect to host via SSH
scp file user@host:path                      # secure file copy to remote host
curl -I url                                  # fetch HTTP headers only
wget url                                     # download file from URL
ip route show                                # show routing table
sudo ip route add 8.8.8.8/32 via 4.4.4.4    # add static route via gateway
sudo ip route add unreachable 8.8.8.8/32    # add unreachable route (returns error)
sudo ip route add blackhole 8.8.8.8/32      # add blackhole route (silently drops)
sudo ip route del 8.8.8.8/32                # delete a route
sudo netplan apply                           # apply netplan network configuration
nslookup <domain>                             # see IP of domain
dig <domain>                                  # more info about Domain
```

> **Flags**
> `-I` show all IPs · `-tulnp` TCP+UDP listening numeric with process · `-I` headers only
> `/32` single host · `via` next hop gateway · `unreachable` returns error · `blackhole` silent drop

> Persistent route via yq:
```bash
sudo yq -i '.network.ethernets.enp0s3.routes = [{"to": "8.8.8.8/32", "via": "4.4.4.4"}]' /etc/netplan/00-installer-config.yaml
```

---

### Hostname Resolution

```bash
nslookup hostname          # query DNS for hostname IP
dig +short hostname        # DNS lookup, short output only
host hostname              # simple DNS lookup
getent hosts hostname      # resolve hostname (checks /etc/hosts first)
sudo nano /etc/hosts       # edit permanent local hostname mappings
```

> **Flags**
> `+short` short output IP only

---

### SSH

```bash
sudo apt install openssh-server   # install SSH server
sudo systemctl start ssh          # start SSH service
sudo systemctl enable ssh         # enable SSH to start on boot
sudo systemctl status ssh         # check SSH service status
```

> **Options**
> `start` start now · `enable` start on boot · `status` show current state

---

### Disk & Memory

```bash
df -h             # show disk space (human-readable)
du -sh dir/       # show size of a directory
free -h           # show RAM and swap usage
lsblk             # list block devices and partitions
mount /dev/sdb /mnt   # mount a disk
iostat            # show disk I/O stats
```

> **Flags**
> `-h` human readable · `-s` summary only

---

### LVM

```bash
sudo pvcreate /dev/sdb                          # create physical volume on disk
sudo vgcreate vg_name /dev/sdb                  # create volume group from physical volume
sudo lvcreate -L 1G -n lv_name vg_name          # create logical volume with fixed size
sudo lvcreate -l 100%FREE -n lv_name vg_name    # create logical volume using all free space
sudo mkfs.ext4 /dev/vg_name/lv_name             # format logical volume as ext4
sudo mount /dev/vg_name/lv_name /mnt/mountpoint # mount logical volume
sudo blkid /dev/vg_name/lv_name                 # get UUID of logical volume
sudo mount -a                                   # mount all entries in /etc/fstab
sudo pvs && sudo vgs && sudo lvs                # list all PVs, VGs and LVs
sudo lvrename vg_name old_lv new_lv             # rename logical volume
sudo vgrename old_vg new_vg                     # rename volume group
```

> **Flags**
> `-L` fixed size e.g. `1G` · `-l` relative size e.g. `100%FREE` · `-n` name of logical volume

> Permanent mount in `/etc/fstab`:
```
UUID=xxxx  /mnt/mountpoint  ext4  defaults  0  0
```

---

### Swap

```bash
sudo fallocate -l 1G /swapfile   # create swap file of given size
sudo chmod 600 /swapfile         # set correct permissions on swap file
sudo mkswap /swapfile            # set up swap area on file
sudo swapon /swapfile            # enable swap file
sudo swapon --show               # show active swap spaces
free -h                          # show memory and swap usage
```

> **Flags**
> `-l` size to allocate · `600` owner read/write only (required for swap)

> Permanent swap in `/etc/fstab`:
```
/swapfile  none  swap  sw  0  0
```

---

### Pipes & Redirects

```bash
cmd1 | cmd2        # pipe output of cmd1 into cmd2
cmd > file.txt     # redirect output to file (overwrite)
cmd >> file.txt    # append output to file
cmd 2>&1           # redirect stderr to stdout
cmd < file.txt     # use file as input
```

> **Symbols**
> `|` pipe · `>` overwrite · `>>` append · `2>&1` merge stderr into stdout

---

### Compression

```bash
tar -czf out.tar.gz dir/                    # create gzip archive
tar -xzf file.tar.gz                        # extract gzip archive
tar -C /home/user -xvf archive.tar.gz       # extract into target folder
zip -r out.zip dir/                         # create zip archive
unzip file.zip                              # extract zip archive
gzip / gunzip file                          # compress / decompress gzip
bzip2 / bunzip2 file                        # compress / decompress bzip2 (smaller than gzip)
tar cjf MyBZIP.bz2 Folder1                  # create bzip2 archive
```

> **Flags**
> `-c` create · `-x` extract · `-z` gzip · `-j` bzip2 · `-f` filename · `-v` verbose · `-C` target directory

---

### Text Processing

```bash
sort file              # sort lines alphabetically
uniq -c                # count unique lines
awk '{print $1}' file  # print first column
sed 's/old/new/g' file # find and replace all occurrences
cut -d: -f1 file       # cut first field by delimiter
tr 'a-z' 'A-Z'         # translate lowercase to uppercase
```

> **Flags**
> `-c` count occurrences · `-d` delimiter · `-f` field number · `s/old/new/g` substitute globally

---

### Docker

```bash
sudo apt install docker-ce docker-ce-cli containerd.io   # install Docker
sudo usermod -aG docker username                         # allow user to run Docker without sudo
apt-cache madison docker-ce                              # list available Docker versions
docker run -d -p 8080:80 --name my-nginx nginx           # run nginx container on port 8080
docker build -t my-nginx .                               # build Docker image from Dockerfile
```

> **Flags**
> `-d` detached/background · `-p 8080:80` host:container port map · `--name` container name · `-t` image tag name

---

### NGINX

```bash
sudo apt install nginx                        # install nginx
sudo systemctl restart nginx                  # restart nginx service
sudo ss -lntp | grep 8080                     # check if nginx is listening on port 8080
curl localhost:8080                           # test nginx response
logrotate -d /etc/logrotate.d/nginx           # test logrotate config (dry run)
```

> **Flags**
> `-lntp` listening+numeric+TCP+process · `-d` dry run test only

---

### Monitoring

```bash
# Processes
top                  # live updating process list (Shift+P CPU, Shift+M memory)
ps aux               # snapshot of all running processes
kill PID             # stop process by PID
killall name         # kill all processes by name

# CPU
lscpu                # CPU architecture info (cores, threads, sockets)
mpstat               # per-CPU usage statistics
sar                  # historical CPU statistics

# Memory
free -h              # show RAM and swap usage
vmstat               # processes, memory, paging, IO, CPU activity

# Storage
df -h                # available disk space
du -sh dir/          # disk usage of directory
lsof                 # list open files
iostat               # disk IO statistics

# Logs
dmesg                         # kernel message buffer
journalctl                    # query systemd journal
/var/log/messages             # global system messages
/var/log/secure               # authentication and login info
/var/log/cron                 # cron job execution logs
/var/log/auth.log             # user logins and auth mechanisms
/var/log/maillog              # mail server logs
```

---

### Aliases

```bash
alias shortname='command'           # create temporary alias (current session only)
sudo nano /home/username/.bashrc    # edit user bashrc for permanent aliases
source ~/.bashrc                    # apply bashrc changes without logout
alias                               # list all current aliases
unalias shortname                   # remove an alias
```

---

### Shortcuts

```bash
Ctrl + C    # kill current process
Ctrl + Z    # suspend process to background
Ctrl + R    # search command history
Ctrl + L    # clear screen
!!          # repeat last command
!cmd        # repeat last command starting with 'cmd'
```

---

### System Info

```bash
lsb_release -d    # show full OS name and version
nproc             # show number of CPUs
free -h           # show RAM and swap usage
date +%s          # show current Unix epoch timestamp
lsblk             # list all block devices and partitions
df -h             # show disk usage of mounted filesystems
```

> **Flags**
> `-d` full OS description · `-h` human readable · `+%s` Unix epoch timestamp

---

### Tools

```bash
sudo wget -qO /usr/local/bin/yq https://github.com/mikefarah/yq/releases/latest/download/yq_linux_amd64   # download yq binary
sudo chmod a+x /usr/local/bin/yq   # make yq executable
```

> **Flags**
> `-qO` quiet + output to file · `a+x` add execute for all users
