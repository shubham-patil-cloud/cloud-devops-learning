# 🐧 Linux Learning — Amazon Linux for AWS/DevOps
## Complete Lesson 1 → Lesson 25

This is now your **AWS-focused Linux course**, using **Amazon Linux on EC2** as the primary environment.  
I'll use **`yum`** for package-management examples.

---

# LESSON 1 — Linux & Amazon Linux Fundamentals

## 1. What is Linux?

Linux is an **open-source operating-system kernel**.

The kernel manages:

- CPU
- Memory
- Processes
- Files
- Hardware
- Networking
- Security

A complete Linux operating system is usually:

```text
Linux Kernel
     +
System Utilities
     +
Libraries
     +
Shell
     +
Applications
     =
Linux Distribution
```

Examples:

- Amazon Linux
- Ubuntu
- Debian
- Red Hat Enterprise Linux
- Rocky Linux
- AlmaLinux

---

## 2. What is Amazon Linux?

**Amazon Linux** is a Linux distribution provided by AWS and designed for AWS workloads.

It is commonly used for:

- EC2
- Web servers
- Application servers
- Docker hosts
- DevOps environments
- AWS automation

For your learning:

```text
AWS EC2
   ↓
Amazon Linux
   ↓
Bash
   ↓
Linux Commands
   ↓
Application
```

---

## 3. Amazon Linux vs Ubuntu

| Feature | Amazon Linux | Ubuntu |
|---|---|---|
| AWS optimized | Yes | Yes |
| Package manager | `yum` | `apt` |
| Package format | RPM | DEB |
| Service manager | systemd | systemd |
| Default EC2 user | `ec2-user` | `ubuntu` |
| SSH service | `sshd` | `ssh` |
| Common firewall | firewalld/nftables | ufw/nftables |
| Enterprise usage | Very common | Very common |

---

## 4. Connect to EC2

Typical Amazon Linux SSH:

```bash
ssh -i my-key.pem ec2-user@PUBLIC_IP
```

Example:

```bash
ssh -i aws-key.pem ec2-user@54.10.20.30
```

---

## 5. First commands

```bash
whoami
hostname
pwd
uname -a
cat /etc/os-release
uptime
```

Useful:

```bash
whoami
```

Shows current user.

```bash
cat /etc/os-release
```

Shows OS information.

---

## 6. Why Linux is important for DevOps

Most cloud servers run Linux.

You need Linux for:

- EC2 administration
- Docker
- Kubernetes
- Jenkins
- GitHub Actions runners
- Terraform
- Ansible
- Nginx
- Apache
- databases
- monitoring

### Interview

**Q: Why is Linux important in DevOps?**

> Linux is widely used for cloud and server workloads, so DevOps engineers need Linux skills for server administration, deployments, networking, security, troubleshooting and automation.

### MUST KNOW

```bash
whoami
hostname
pwd
uname -a
cat /etc/os-release
uptime
```

---

# LESSON 2 — Linux Architecture on Amazon EC2

## Linux architecture

```text
Applications
     ↓
Shell / System Utilities
     ↓
System Libraries
     ↓
Linux Kernel
     ↓
Hardware
```

On AWS:

```text
AWS EC2
   ↓
Virtual Hardware
   ↓
Amazon Linux Kernel
   ↓
systemd
   ↓
Services
   ↓
Applications
```

---

## Kernel

The kernel manages:

### CPU

Schedules processes.

### Memory

Allocates RAM.

### Filesystem

Manages files and directories.

### Network

Manages network interfaces and communication.

### Devices

Communicates with hardware/block devices.

---

## User space vs kernel space

```text
USER SPACE
Applications
Bash
Nginx
Python
Git
     ↓
SYSTEM CALL
     ↓
KERNEL SPACE
     ↓
CPU / Memory / Disk / Network
```

---

## Shell

The shell allows you to communicate with Linux.

Amazon Linux commonly uses:

```bash
/bin/bash
```

Check:

```bash
echo $SHELL
```

---

## systemd

Modern Amazon Linux uses **systemd** for service and system management.

Example:

```bash
systemctl status nginx
```

---

## Interview

**Q: What is the Linux kernel?**

> The kernel is the core component of Linux that manages CPU, memory, processes, devices, filesystems and networking.

**Q: What is Bash?**

> Bash is a command-line shell and scripting environment used to interact with Linux.

### MUST KNOW

```text
Application
↓
Shell
↓
System Libraries
↓
Kernel
↓
Hardware
```

---

# LESSON 3 — Amazon Linux Filesystem

Linux has a single filesystem hierarchy beginning at:

```text
/
```

This is called the **root directory**.

---

## Important directories

| Directory | Purpose |
|---|---|
| `/` | Root |
| `/home` | Normal users |
| `/root` | Root user's home |
| `/etc` | Configuration |
| `/var` | Variable data |
| `/var/log` | Logs |
| `/tmp` | Temporary files |
| `/opt` | Optional applications |
| `/usr` | Programs/libraries |
| `/bin` | Essential commands |
| `/sbin` | System commands |
| `/boot` | Boot files |
| `/dev` | Device files |
| `/proc` | Process/kernel information |
| `/sys` | Kernel/device information |
| `/run` | Runtime information |
| `/mnt` | Temporary mount point |

---

## Amazon Linux important locations

Configuration:

```bash
/etc
```

Logs:

```bash
/var/log
```

User home:

```bash
/home/ec2-user
```

Root home:

```bash
/root
```

Nginx files:

```bash
/etc/nginx
/var/log/nginx
/usr/share/nginx/html
```

---

## Absolute path

Starts from `/`.

```bash
/etc/nginx/nginx.conf
```

## Relative path

Starts from your current directory.

```bash
./script.sh
```

---

## Special paths

```text
.     current directory
..    parent directory
~     current user's home
```

Commands:

```bash
cd /
cd ~
cd ..
cd -
```

### MUST KNOW

```bash
pwd
ls
cd
cd ..
cd ~
```

---

# LESSON 4 — Essential Amazon Linux Commands

## Navigation

```bash
pwd
ls
ls -l
ls -a
ls -la
cd
cd ..
cd ~
cd -
```

---

## Create

```bash
mkdir project
mkdir -p project/app/logs
touch app.log
```

---

## Copy

```bash
cp file1 file2
cp -r directory1 directory2
```

---

## Move / rename

```bash
mv old.txt new.txt
mv app.txt /tmp/
```

---

## Delete

```bash
rm file.txt
rm -r directory
rm -f file
rm -rf directory
```

⚠️ `rm -rf` is dangerous.

---

## Read files

```bash
cat file.txt
less file.txt
head file.txt
tail file.txt
```

Real-world:

```bash
tail -f /var/log/nginx/access.log
```

---

## Output

```bash
echo "Hello"
echo "Hello" > file.txt
echo "New line" >> file.txt
```

Difference:

```text
>   overwrite
>>  append
```

---

## Information

```bash
file file.txt
stat file.txt
history
man ls
```

### Practical lab

```bash
mkdir -p ~/devops/app/logs
cd ~/devops
touch app.txt
echo "Amazon Linux" > app.txt
cat app.txt
cp app.txt backup.txt
ls -la
```

---

# LESSON 5 — Files & Directory Management

## File types

Check:

```bash
ls -l
```

First character:

```text
-  regular file
d  directory
l  symbolic link
b  block device
c  character device
p  pipe
s  socket
```

---

## Find files

```bash
find /home -name "*.log"
```

Examples:

```bash
find /var/log -type f
find /var/log -type f -name "*.log"
find /var -type f -size +500M
find /tmp -type f -mtime +7
```

---

## `which`

```bash
which nginx
which bash
```

---

## `whereis`

```bash
whereis nginx
```

---

## Symbolic link

```bash
ln -s /var/log/nginx/access.log access.log
```

Check:

```bash
ls -l access.log
```

---

## Hard link

```bash
ln file1 file2
```

Basic idea:

```text
Hard link → same inode
Symbolic link → path/reference
```

---

## AWS relevance

You will frequently manage:

```text
/opt/app
/var/www
/var/log
/etc/nginx
/home/ec2-user
```

### MUST KNOW

```bash
find
ls -l
file
stat
ln -s
cp -r
mv
rm -rf
```

---

# LESSON 6 — Permissions & Ownership

Linux permissions are:

```text
r = read
w = write
x = execute
```

Three categories:

```text
User
Group
Others
```

Example:

```text
-rwxr-xr--
```

Breakdown:

```text
- | rwx | r-x | r--
    user  group others
```

---

## Numeric permissions

```text
r = 4
w = 2
x = 1
```

Therefore:

```text
7 = rwx
6 = rw-
5 = r-x
4 = r--
```

---

## Common permissions

### 755

```text
rwxr-xr-x
```

### 644

```text
rw-r--r--
```

### 700

```text
rwx------
```

### 600

```text
rw-------
```

---

## chmod

```bash
chmod 755 script.sh
chmod 644 index.html
chmod 700 private.sh
chmod 600 secret.txt
```

Symbolic:

```bash
chmod u+x script.sh
chmod g+r file
chmod o-w file
chmod a+x script.sh
```

---

## Ownership

```bash
chown user file
chown user:group file
chgrp group file
```

Recursive:

```bash
chown -R ec2-user:developers /opt/app
```

---

## sudo

```bash
sudo command
```

Example:

```bash
sudo yum install nginx -y
```

---

## Amazon Linux `wheel`

Administrators commonly use the `wheel` group for sudo access.

```bash
sudo usermod -aG wheel devops
```

---

## SSH key

Private key should be protected:

```bash
chmod 400 my-key.pem
```

---

## Why not `777`?

```bash
chmod 777 file
```

gives everyone read/write/execute access.

Avoid it unless there is a specific, justified requirement.

### MUST KNOW

```bash
ls -l
chmod
chown
chgrp
sudo
```

---

# LESSON 7 — Users & Groups

## Check current user

```bash
whoami
id
groups
```

---

## Create user

```bash
sudo useradd devops
```

Create home directory:

```bash
sudo useradd -m devops
```

Create with Bash:

```bash
sudo useradd -m -s /bin/bash devops
```

---

## Password

```bash
sudo passwd devops
```

---

## Groups

```bash
sudo groupadd developers
```

Add user:

```bash
sudo usermod -aG developers devops
```

Important:

```text
-a = append
-G = supplementary groups
```

Without `-a`, you can accidentally replace supplementary group membership.

---

## Check

```bash
id devops
groups devops
```

---

## User files

```bash
/etc/passwd
/etc/shadow
/etc/group
```

---

## Switch user

```bash
su - devops
```

---

## Delete

```bash
sudo userdel devops
```

With home directory:

```bash
sudo userdel -r devops
```

---

## Amazon Linux sudo

```bash
sudo visudo
```

Configuration can also be placed under:

```text
/etc/sudoers.d/
```

---

## Practical AWS scenario

Create:

```text
devops
developers
```

Then:

```bash
sudo usermod -aG developers devops
sudo usermod -aG wheel devops
```

Mental model:

```text
USER
 ↓
GROUP
 ↓
OWNERSHIP
 ↓
PERMISSIONS
 ↓
ACCESS
```

---

# LESSON 8 — YUM & RPM

Amazon Linux package management is centered around RPM-based tooling.

## YUM

Install:

```bash
sudo yum install nginx -y
```

Update:

```bash
sudo yum update -y
```

Remove:

```bash
sudo yum remove nginx
```

Search:

```bash
yum search nginx
```

Information:

```bash
yum info nginx
```

Installed packages:

```bash
yum list installed
```

Repositories:

```bash
yum repolist
```

---

## RPM

Check package:

```bash
rpm -q nginx
```

All installed packages:

```bash
rpm -qa
```

Files installed by package:

```bash
rpm -ql nginx
```

---

## YUM vs RPM

```text
YUM
 ↓
Repository
 ↓
Package
 ↓
Dependencies
 ↓
Installation
```

RPM works more directly with RPM packages.

### Example

```bash
sudo yum install nginx -y
```

Then:

```bash
rpm -q nginx
```

### Interview

**Q: Why use YUM instead of RPM?**

> YUM works with repositories and resolves package dependencies automatically, while RPM is the lower-level package-management tool.

---

# LESSON 9 — Processes & Services

## Process

A running program is a **process**.

Every process has a PID.

```bash
ps
ps aux
```

Find process:

```bash
pgrep -a nginx
```

---

## top

```bash
top
```

Useful for:

- CPU
- memory
- processes
- load

---

## Kill process

```bash
kill PID
```

Force:

```bash
kill -9 PID
```

Use `-9` only when necessary.

---

## Background

```bash
sleep 100 &
```

Check:

```bash
jobs
```

Foreground:

```bash
fg
```

Background:

```bash
bg
```

---

# systemd services

Amazon Linux uses systemd.

```bash
systemctl status nginx
```

Start:

```bash
sudo systemctl start nginx
```

Stop:

```bash
sudo systemctl stop nginx
```

Restart:

```bash
sudo systemctl restart nginx
```

Reload:

```bash
sudo systemctl reload nginx
```

Enable at boot:

```bash
sudo systemctl enable nginx
```

Enable + start:

```bash
sudo systemctl enable --now nginx
```

---

## Logs

```bash
journalctl
journalctl -u nginx
journalctl -u nginx -f
```

---

## Process vs service

```text
kill
 ↓
Process

systemctl
 ↓
Service
```

### MUST KNOW

```bash
ps aux
top
pgrep
kill
systemctl
journalctl
```

---

# LESSON 10 — Amazon Linux Networking Basics

## IP address

An EC2 instance normally has:

- private IPv4
- public IPv4 when configured
- network interface

Check:

```bash
ip a
```

---

## Routing

```bash
ip route
```

---

## Ports

Examples:

```text
22   SSH
80   HTTP
443  HTTPS
8080 Application
3306 MySQL
5432 PostgreSQL
```

Check listening ports:

```bash
ss -lntp
```

---

## Ping

```bash
ping 8.8.8.8
```

---

## Curl

```bash
curl http://localhost
```

Headers:

```bash
curl -I http://localhost
```

---

## DNS

```bash
nslookup google.com
dig google.com
```

---

## EC2 networking

```text
Internet
   ↓
Internet Gateway
   ↓
Route Table
   ↓
Subnet
   ↓
EC2 ENI
   ↓
Amazon Linux
```

---

# LESSON 11 — Advanced Linux Networking

## Interface

```bash
ip link
```

Statistics:

```bash
ip -s link
```

---

## Route

```bash
ip route
```

Specific route:

```bash
ip route get 8.8.8.8
```

---

## TCP connection

```bash
ss -tn
```

Listening:

```bash
ss -lntp
```

Established:

```bash
ss -tn state established
```

Summary:

```bash
ss -s
```

---

## Port test

```bash
nc -zv localhost 8080
```

---

## Localhost

```text
127.0.0.1
```

means the local machine.

If an application listens on:

```text
127.0.0.1:8080
```

remote machines cannot directly connect to it.

If it listens on:

```text
0.0.0.0:8080
```

it can accept connections on available IPv4 interfaces, subject to firewall/network rules.

---

## DNS

```bash
cat /etc/resolv.conf
```

Hosts:

```bash
cat /etc/hosts
```

---

## TCP packet capture

```bash
sudo tcpdump -i any port 80
```

Numeric mode:

```bash
sudo tcpdump -nn -i any port 80
```

Save:

```bash
sudo tcpdump -nn -i any port 80 -w capture.pcap
```

---

## AWS troubleshooting layers

```text
Application
 ↓
Port
 ↓
Linux Firewall
 ↓
Security Group
 ↓
NACL
 ↓
Route Table
 ↓
Subnet
 ↓
VPC
 ↓
Internet
```

---

# LESSON 12 — Bash Scripting Fundamentals

Create:

```bash
nano server_info.sh
```

Script:

```bash
#!/bin/bash

echo "Amazon Linux Server"
echo "Hostname: $(hostname)"
echo "User: $(whoami)"
echo "Uptime:"
uptime

echo "Memory:"
free -h

echo "Disk:"
df -h
```

Make executable:

```bash
chmod +x server_info.sh
```

Run:

```bash
./server_info.sh
```

---

## Variables

```bash
APP_NAME="myapp"
echo "$APP_NAME"
```

---

## Arguments

```bash
echo "$0"
echo "$1"
echo "$2"
```

Run:

```bash
./script.sh hello world
```

---

## Conditions

```bash
if [ -f /etc/nginx/nginx.conf ]; then
    echo "Nginx configuration exists"
else
    echo "Not found"
fi
```

---

## Loops

```bash
for server in web1 web2 web3
do
    echo "$server"
done
```

---

## Function

```bash
check_disk() {
    df -h
}

check_disk
```

---

# LESSON 13 — Text Processing & Logs

## grep

```bash
grep "ERROR" app.log
```

Useful:

```bash
grep -i "error" app.log
grep -n "error" app.log
grep -v "success" app.log
grep -c "ERROR" app.log
grep -r "ERROR" /var/log
```

---

## Nginx logs

```text
/var/log/nginx/access.log
/var/log/nginx/error.log
```

---

## tail

```bash
tail -f /var/log/nginx/access.log
```

---

## awk

Get first field:

```bash
awk '{print $1}' access.log
```

Count IPs:

```bash
awk '{print $1}' access.log | sort | uniq -c
```

Highest:

```bash
awk '{print $1}' access.log | sort | uniq -c | sort -nr
```

---

## cut

```bash
cut -d: -f1 /etc/passwd
```

---

## sort

```bash
sort file.txt
sort -n numbers.txt
sort -r file.txt
```

---

## uniq

```bash
sort file.txt | uniq
sort file.txt | uniq -c
```

---

## sed

```bash
sed 's/old/new/' file.txt
```

Global:

```bash
sed 's/old/new/g' file.txt
```

---

## wc

```bash
wc -l file.txt
wc -w file.txt
```

---

## Amazon Linux logs

Common examples include:

```text
/var/log/messages
/var/log/secure
/var/log/cron
/var/log/nginx/
```

systemd:

```bash
journalctl -u sshd
journalctl -u nginx
```

Mental model:

```text
RAW LOG
 ↓
grep
 ↓
awk/cut
 ↓
sort
 ↓
uniq -c
 ↓
sort -nr
 ↓
ANALYSIS
```

---

# LESSON 14 — Amazon Linux Troubleshooting

## Golden troubleshooting method

```text
CHECK
 ↓
IDENTIFY
 ↓
INVESTIGATE
 ↓
FIX
 ↓
VERIFY
 ↓
MONITOR
 ↓
PREVENT
```

---

## CPU

```bash
uptime
top
ps aux --sort=-%cpu | head
```

---

## Memory

```bash
free -h
ps aux --sort=-%mem | head
```

---

## Disk

```bash
df -h
df -i
du -sh /*
```

Find large files:

```bash
sudo find /var -type f -size +500M
```

---

## Service

```bash
systemctl status nginx
journalctl -u nginx -n 50
```

---

## Nginx configuration

```bash
sudo nginx -t
```

---

## Port

```bash
ss -lntp
```

---

## Network

```bash
ping
curl
dig
ip a
ip route
```

---

## Example: Website down

Check:

```bash
systemctl status nginx
```

Then:

```bash
ss -lntp
```

Then:

```bash
curl http://localhost
```

Then:

```bash
nginx -t
```

Then:

```bash
journalctl -u nginx
```

Then investigate:

```text
Security Group
Linux Firewall
Route
DNS
Nginx
Application
```

---

# LESSON 15 — Amazon Linux Security

## Authentication vs Authorization

```text
Authentication
= Who are you?

Authorization
= What are you allowed to do?
```

---

## sudo

```bash
sudo -l
```

Configuration:

```bash
sudo visudo
```

---

## SSH security

Configuration:

```bash
/etc/ssh/sshd_config
```

Important settings:

```text
PermitRootLogin
PasswordAuthentication
PubkeyAuthentication
Port
```

Validate:

```bash
sudo sshd -t
```

Restart:

```bash
sudo systemctl restart sshd
```

---

## Firewall

Check:

```bash
sudo systemctl status firewalld
```

State:

```bash
sudo firewall-cmd --state
```

List:

```bash
sudo firewall-cmd --list-all
```

---

## SELinux

Check:

```bash
getenforce
sestatus
```

Modes:

```text
Enforcing
Permissive
Disabled
```

---

## Special permissions

SUID:

```text
4
```

SGID:

```text
2
```

Sticky bit:

```text
1
```

Example:

```bash
chmod 4755 file
chmod 2755 directory
chmod 1777 directory
```

---

## AWS security layers

```text
Internet
 ↓
Security Group
 ↓
EC2
 ↓
Linux Firewall
 ↓
Application
```

Security Group is **AWS-level**.

Linux firewall is **OS-level**.

---

# LESSON 16 — EBS & Amazon Linux Storage

This is one of the most important AWS/Linux topics.

## Storage architecture

```text
EBS
 ↓
Block Device
 ↓
Partition
 ↓
Filesystem
 ↓
Mount Point
 ↓
Files
```

---

## Check disks

```bash
lsblk
```

Filesystem:

```bash
lsblk -f
```

Usage:

```bash
df -h
```

---

## Example

Suppose EBS appears as:

```text
/dev/xvdf
```

Partition:

```bash
sudo fdisk /dev/xvdf
```

Create partition using:

```text
n
p
1
Enter
Enter
w
```

Check:

```bash
lsblk
```

---

## Create XFS

```bash
sudo mkfs.xfs /dev/xvdf1
```

---

## Mount

```bash
sudo mkdir /data
sudo mount /dev/xvdf1 /data
```

Check:

```bash
df -h
findmnt /data
```

---

## UUID

```bash
sudo blkid /dev/xvdf1
```

Add UUID to:

```text
/etc/fstab
```

Example:

```text
UUID=xxxx-xxxx /data xfs defaults,nofail 0 0
```

Test:

```bash
sudo mount -a
```

---

## Disk expansion

If AWS EBS is increased:

```text
10 GB
 ↓
20 GB
```

Linux may still show 10 GB.

Check:

```bash
lsblk
df -h
```

If partition must grow:

```bash
sudo growpart /dev/xvdf 1
```

XFS:

```bash
sudo xfs_growfs /data
```

ext4:

```bash
sudo resize2fs /dev/xvdf1
```

Verify:

```bash
df -h
```

---

## Important distinction

```text
EBS Volume
   ↓
Partition
   ↓
Filesystem
```

Growing one does not always automatically grow the others.

---

# LESSON 17 — Amazon Linux Performance Monitoring

## CPU

```bash
top
```

CPU count:

```bash
nproc
```

Detailed:

```bash
lscpu
```

---

## Load average

```bash
uptime
```

Example:

```text
load average: 1.20, 0.80, 0.50
```

Represents approximately:

```text
1 minute
5 minutes
15 minutes
```

Interpret relative to available CPU cores and whether the workload is CPU- or I/O-bound.

---

## Memory

```bash
free -h
```

Swap:

```bash
swapon --show
```

---

## vmstat

```bash
vmstat 2
```

Useful for:

- CPU
- memory
- swap
- processes
- I/O

---

## Disk I/O

Install sysstat if required:

```bash
sudo yum install sysstat -y
```

Then:

```bash
iostat -xz 2
```

Important metrics:

```text
%util
await
r/s
w/s
```

---

## Network

```bash
ip -s link
ss -s
ss -tn
```

---

## AWS connection

```text
EC2 Linux
   ↓
CPU / Memory / Disk / Network
   ↓
CloudWatch
   ↓
Metrics
   ↓
Alarm
   ↓
Action
```

---

## Performance troubleshooting

```text
uptime
 ↓
top
 ↓
free -h
 ↓
ps
 ↓
df -h
 ↓
iostat
 ↓
ss
 ↓
logs
 ↓
application
```

---

# LESSON 18 — Cron & Scheduling

Amazon Linux uses:

```text
crond
```

Check:

```bash
systemctl status crond
```

Enable:

```bash
sudo systemctl enable --now crond
```

---

## Crontab

```bash
crontab -l
crontab -e
```

Syntax:

```text
* * * * * command
│ │ │ │ │
│ │ │ │ └── day of week
│ │ │ └──── month
│ │ └────── day
│ └──────── hour
└────────── minute
```

---

## Examples

Every minute:

```text
* * * * *
```

Every hour:

```text
0 * * * *
```

Daily at 2 AM:

```text
0 2 * * *
```

Every 15 minutes:

```text
*/15 * * * *
```

Monday-Friday:

```text
* * * * 1-5
```

---

## Cron + script

```bash
/home/ec2-user/scripts/backup.sh
```

Better:

```text
0 2 * * * /home/ec2-user/scripts/backup.sh >> /home/ec2-user/backup.log 2>&1
```

---

## Cron environment

Cron may have a different environment/PATH.

Therefore:

- use absolute paths
- test manually
- redirect output
- don't assume your interactive shell environment

---

## AWS comparison

```text
Linux Cron
    ↓
Inside EC2

AWS Scheduled Scaling/EventBridge
    ↓
AWS infrastructure
```

---

# LESSON 19 — Environment Variables

## Variable

```bash
APP_ENV=production
```

Shell variable.

Export:

```bash
export APP_ENV=production
```

Now child processes can access it.

```bash
echo "$APP_ENV"
```

---

## Important variables

```bash
echo "$HOME"
echo "$USER"
echo "$SHELL"
echo "$PATH"
echo "$PWD"
echo "$HOSTNAME"
```

---

## Environment

```bash
env
printenv
```

---

## PATH

```bash
echo "$PATH"
```

Temporarily:

```bash
export PATH="$PATH:/home/ec2-user/scripts"
```

---

## Bash configuration

```text
~/.bashrc
~/.bash_profile
/etc/profile
/etc/environment
```

Apply:

```bash
source ~/.bashrc
```

---

## Exit status

```bash
echo $?
```

Typically:

```text
0 = success
non-zero = failure
```

---

## Defaults

```bash
APP_ENV="${APP_ENV:-development}"
```

Required:

```bash
: "${DB_HOST:?DB_HOST is required}"
```

---

## AWS best practice

Don't hardcode AWS credentials in:

```text
scripts
environment files
GitHub repositories
```

For EC2 workloads, prefer an **IAM role attached to the instance** when appropriate.

---

# LESSON 20 — Advanced Bash Automation

## Arguments

```text
$0 = script
$1 = first argument
$2 = second
$# = argument count
$@ = all arguments
```

Example:

```bash
#!/bin/bash

echo "Script: $0"
echo "First argument: $1"
echo "Arguments: $#"
```

---

## Arrays

```bash
servers=("web1" "web2" "web3")
echo "${servers[0]}"
```

---

## Case

```bash
case "$1" in
    start)
        echo "Starting"
        ;;
    stop)
        echo "Stopping"
        ;;
    *)
        echo "Usage: $0 {start|stop}"
        ;;
esac
```

---

## Safe Bash

Use:

```bash
set -euo pipefail
```

Meaning broadly:

```text
-e → stop on command failure
-u → catch unset variables
-o pipefail → catch failures inside pipelines
```

---

## Debug

Syntax:

```bash
bash -n script.sh
```

Debug execution:

```bash
bash -x script.sh
```

---

## trap

```bash
trap 'echo "Script interrupted"' INT
```

---

## Real DevOps automation

You should be able to create:

```text
health_check.sh
backup.sh
disk_monitor.sh
service_monitor.sh
log_analyzer.sh
deploy.sh
```

Mental model:

```text
Bash
 ↓
Commands
 ↓
Conditions
 ↓
Loops
 ↓
Functions
 ↓
Exit Codes
 ↓
Error Handling
 ↓
Logging
 ↓
Automation
```

---

# LESSON 21 — SSH & Secure Remote Administration

## SSH

SSH provides secure remote access.

Default:

```text
TCP 22
```

---

## Amazon Linux

```bash
ssh -i my-key.pem ec2-user@PUBLIC_IP
```

Ubuntu:

```bash
ssh -i my-key.pem ubuntu@PUBLIC_IP
```

---

## Private key permissions

```bash
chmod 400 my-key.pem
```

---

## SSH server

Amazon Linux:

```bash
systemctl status sshd
```

---

## SSH configuration

```text
/etc/ssh/sshd_config
```

Validate:

```bash
sudo sshd -t
```

---

## Authorized keys

```text
~/.ssh/authorized_keys
```

Known hosts:

```text
~/.ssh/known_hosts
```

---

## SCP

Upload:

```bash
scp -i key.pem app.py ec2-user@SERVER:/home/ec2-user/
```

---

## Remote command

```bash
ssh -i key.pem ec2-user@SERVER "uptime"
```

---

## SSH debugging

```bash
ssh -v -i key.pem ec2-user@SERVER
```

More:

```bash
ssh -vvv -i key.pem ec2-user@SERVER
```

---

## Jump host

```bash
ssh -J bastion-user@BASTION internal-user@PRIVATE-IP
```

---

## AWS SSH troubleshooting

```text
Client
 ↓
Internet
 ↓
Security Group :22
 ↓
NACL
 ↓
Route
 ↓
EC2
 ↓
Linux Firewall
 ↓
sshd
 ↓
Authentication
```

---

# LESSON 22 — Amazon Linux Network Troubleshooting

## Scenario: Port 8080 doesn't work

### Step 1 — Is application running?

```bash
ps aux
```

### Step 2 — Is port listening?

```bash
ss -lntp
```

### Step 3 — Is application reachable locally?

```bash
curl http://localhost:8080
```

### Step 4 — Test TCP

```bash
nc -zv localhost 8080
```

### Step 5 — Check firewall

```bash
sudo firewall-cmd --list-all
```

### Step 6 — Check AWS Security Group

Verify inbound rule.

### Step 7 — Check route

```bash
ip route
```

### Step 8 — Capture packets

```bash
sudo tcpdump -nn -i any port 8080
```

---

## Scenario: DNS problem

```bash
getent hosts example.com
nslookup example.com
dig example.com
```

---

## Scenario: Nginx 502

```text
Client
 ↓
Nginx
 ↓
Application :8080
```

Check:

```bash
systemctl status nginx
ss -lntp
curl http://127.0.0.1:8080
journalctl -u nginx
```

---

## Scenario: EC2 has no Internet

Check:

```bash
ip a
ip route
ping 8.8.8.8
dig google.com
```

Then AWS:

```text
Subnet
 ↓
Route Table
 ↓
Internet Gateway/NAT
 ↓
Security Group
 ↓
NACL
```

---

# LESSON 23 — Nginx on Amazon Linux

## Install

```bash
sudo yum install nginx -y
```

Start:

```bash
sudo systemctl enable --now nginx
```

Check:

```bash
systemctl status nginx
```

Version:

```bash
nginx -v
```

---

## Configuration

Main:

```text
/etc/nginx/nginx.conf
```

HTML:

```text
/usr/share/nginx/html/
```

Logs:

```text
/var/log/nginx/
```

---

## Test

```bash
curl http://localhost
```

Port:

```bash
ss -lntp
```

---

## Configuration validation

```bash
sudo nginx -t
```

---

## Reverse proxy

Architecture:

```text
Internet
   ↓
EC2
   ↓
Nginx :80
   ↓
Reverse Proxy
   ↓
Application :8080
```

Example concept:

```nginx
location / {
    proxy_pass http://127.0.0.1:8080;
}
```

Useful headers:

```nginx
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;
```

---

## 502

Usually investigate:

```text
Nginx
 ↓
Backend
 ↓
Port 8080
 ↓
Application
```

Check:

```bash
curl http://127.0.0.1:8080
ss -lntp
journalctl -u nginx
```

---

## AWS architecture

```text
Internet
 ↓
Security Group
 ↓
EC2
 ↓
Linux Firewall
 ↓
Nginx
 ↓
Application
 ↓
Database
```

---

# LESSON 24 — Amazon Linux Automation & DevOps Administration

## Automation architecture

```text
SCRIPT
 ↓
SCHEDULER
 ↓
EXECUTION
 ↓
LOG
 ↓
MONITOR
 ↓
ALERT
```

---

## Health check

Example:

```bash
#!/bin/bash

if systemctl is-active --quiet nginx; then
    echo "Nginx is healthy"
else
    echo "Nginx is DOWN"
    exit 1
fi
```

---

## Service recovery

```bash
if ! systemctl is-active --quiet nginx; then
    sudo systemctl restart nginx
fi
```

Be careful with automatic restarts in production; repeated restarts can hide deeper problems.

---

## Disk monitoring

Concept:

```bash
df -P / | awk 'NR==2 {print $5}'
```

Then compare against threshold.

---

## Backup

Example:

```bash
tar -czf backup.tar.gz /opt/app
```

---

## Retention

Concept:

```bash
find /backup -type f -mtime +7 -delete
```

---

## Deployment automation

```text
GitHub
 ↓
Pull Code
 ↓
Backup
 ↓
Deploy
 ↓
Install Dependencies
 ↓
Restart
 ↓
Health Check
 ↓
Success
       ↓
   Failure
       ↓
    Rollback
```

---

## Bash vs Ansible

### Bash

Good for:

- simple scripts
- server checks
- local automation

### Ansible

Better for:

- multiple servers
- configuration management
- repeatable infrastructure configuration
- idempotent automation

This becomes important in your later **Ansible** section.

---

# LESSON 25 — FINAL AMAZON LINUX + AWS PROJECT

# 🚀 Production-Style Amazon Linux EC2 Server

This final project combines Lessons 1–24.

---

## Architecture

```text
                         INTERNET
                             |
                             ↓
                    AWS SECURITY GROUP
                       |          |
                    SSH 22      HTTP 80
                       |          |
                       ↓          ↓
                    EC2 INSTANCE
                         |
                   AMAZON LINUX
                         |
       ┌─────────────────┼─────────────────┐
       ↓                 ↓                 ↓
     USERS           FIREWALL           STORAGE
       |                 |                 |
       ↓                 ↓                 ↓
   devops/user          80                EBS
   developers                              |
                                           ↓
                                         /data
                                           |
                         ┌─────────────────┤
                         ↓                 ↓
                       /app              /backup
                         |
                         ↓
                       Nginx
                         |
                         ↓
                  Reverse Proxy
                         |
                         ↓
                    APP :8080
                         |
                         ↓
                       LOGS
                         |
                         ↓
                     CRON
                         |
            ┌────────────┼────────────┐
            ↓            ↓            ↓
        Health Check   Backup    Service Monitor
```

---

# Final Project Steps

## 1. Launch Amazon Linux EC2

Use:

```text
Amazon Linux
```

Connect:

```bash
ssh -i key.pem ec2-user@PUBLIC_IP
```

---

## 2. Update

```bash
sudo yum update -y
```

---

## 3. Create DevOps user

```bash
sudo useradd -m -s /bin/bash devops
sudo passwd devops
```

---

## 4. Create group

```bash
sudo groupadd developers
sudo usermod -aG developers devops
sudo usermod -aG wheel devops
```

Verify:

```bash
id devops
```

---

## 5. Secure SSH key

```bash
chmod 400 key.pem
```

---

## 6. Install Nginx

```bash
sudo yum install nginx -y
```

Start:

```bash
sudo systemctl enable --now nginx
```

---

## 7. Verify

```bash
systemctl status nginx
ss -lntp
curl http://localhost
```

---

## 8. Website

Create:

```bash
sudo nano /usr/share/nginx/html/index.html
```

Example:

```html
<h1>Amazon Linux DevOps Server</h1>
<p>Running on AWS EC2</p>
```

Test:

```bash
curl http://localhost
```

---

## 9. Attach EBS

After attaching EBS:

```bash
lsblk
```

Identify the correct device carefully.

Example:

```text
/dev/xvdf
```

---

## 10. Partition

```bash
sudo fdisk /dev/xvdf
```

Then:

```text
n
p
1
Enter
Enter
w
```

Check:

```bash
lsblk
```

---

## 11. XFS

```bash
sudo mkfs.xfs /dev/xvdf1
```

---

## 12. Mount

```bash
sudo mkdir /data
sudo mount /dev/xvdf1 /data
```

Verify:

```bash
df -h
findmnt /data
```

---

## 13. Persistent mount

```bash
sudo blkid /dev/xvdf1
```

Add UUID to:

```text
/etc/fstab
```

Example:

```text
UUID=xxxx /data xfs defaults,nofail 0 0
```

Test:

```bash
sudo mount -a
```

---

## 14. Application directories

```bash
sudo mkdir -p /data/app
sudo mkdir -p /data/backup
sudo mkdir -p /data/logs
```

---

## 15. Backend application

Run an application on:

```text
127.0.0.1:8080
```

Verify:

```bash
ss -lntp
curl http://127.0.0.1:8080
```

---

## 16. Nginx reverse proxy

```text
Client
 ↓
Nginx :80
 ↓
127.0.0.1:8080
```

Validate:

```bash
sudo nginx -t
```

Reload:

```bash
sudo systemctl reload nginx
```

---

## 17. Logs

Nginx:

```bash
sudo tail -f /var/log/nginx/access.log
```

Errors:

```bash
sudo tail -f /var/log/nginx/error.log
```

Service:

```bash
sudo journalctl -u nginx
```

---

## 18. System monitoring

```bash
uptime
top
free -h
df -h
df -i
ss -s
```

---

## 19. Health-check script

Create:

```text
health_check.sh
```

Check:

```text
CPU
Memory
Disk
Nginx
Application
```

---

## 20. Backup

Create automated backup:

```text
/data/app
        ↓
tar
        ↓
/data/backup
```

---

## 21. Cron

Edit:

```bash
crontab -e
```

Example:

```text
0 2 * * * /home/ec2-user/scripts/backup.sh >> /home/ec2-user/backup.log 2>&1
```

---

## 22. Service monitor

Check Nginx:

```bash
systemctl is-active nginx
```

If required, trigger recovery.

---

# 🔥 Final Break/Fix Scenarios

These are especially important for your **Cloud Support / DevOps interviews**.

---

## Scenario 1 — Website is down

Think:

```text
EC2
 ↓
Nginx
 ↓
Port 80
 ↓
Firewall
 ↓
Security Group
 ↓
Application
```

Commands:

```bash
systemctl status nginx
ss -lntp
curl http://localhost
sudo nginx -t
journalctl -u nginx
```

---

# Scenario 2 — Nginx gives 502

Think:

```text
Nginx
 ↓
Backend :8080
```

Check:

```bash
curl http://127.0.0.1:8080
ss -lntp
ps aux
journalctl -u nginx
```

---

# Scenario 3 — Disk is full

```bash
df -h
df -i
du -sh /*
```

Find large files:

```bash
sudo find /var -type f -size +500M
```

Check deleted-but-open files:

```bash
sudo lsof +L1
```

---

# Scenario 4 — CPU is high

```bash
uptime
top
ps aux --sort=-%cpu | head
```

Identify process → investigate → fix → verify.

---

# Scenario 5 — SSH timeout

Check:

```text
Security Group :22
 ↓
NACL
 ↓
Route
 ↓
EC2
 ↓
Linux Firewall
 ↓
sshd
```

On server:

```bash
systemctl status sshd
ss -lntp
journalctl -u sshd
```

---

# Scenario 6 — Permission denied

Check:

```bash
whoami
id
ls -l
```

Then:

```bash
chmod
chown
groups
sudo
```

---

# Scenario 7 — EBS increased but Linux still shows old size

```bash
lsblk
df -h
```

Then, if needed:

```bash
growpart
```

Then filesystem expansion:

```bash
xfs_growfs
```

or:

```bash
resize2fs
```

Finally:

```bash
df -h
```

---

# 🎯 AMAZON LINUX MUST-KNOW COMMANDS

## System

```bash
uname
cat /etc/os-release
hostname
hostnamectl
uptime
whoami
```

## Files

```bash
pwd
ls
cd
mkdir
touch
cp
mv
rm
find
```

## Text

```bash
grep
awk
sed
cut
sort
uniq
wc
tail
head
```

## Permissions

```bash
chmod
chown
chgrp
ls -l
```

## Users

```bash
id
groups
useradd
usermod
userdel
passwd
groupadd
```

## Packages

```bash
yum
rpm
```

## Processes

```bash
ps
top
pgrep
kill
pstree
```

## Services

```bash
systemctl
journalctl
```

## Networking

```bash
ip
ss
ping
curl
wget
nc
dig
nslookup
traceroute
tcpdump
```

## Storage

```bash
lsblk
df
du
blkid
fdisk
mount
umount
findmnt
```

## Bash

```bash
bash
chmod +x
$?
$0
$1
$@
$#
```

## Scheduling

```bash
crontab
at
systemctl list-timers
```

## Security

```bash
sudo
visudo
sshd -t
firewall-cmd
getenforce
```

## Web server

```bash
nginx
nginx -t
curl
ss
```

---

# 🧠 FINAL AMAZON LINUX MENTAL MODEL

```text
                 AWS
                  ↓
                 EC2
                  ↓
            AMAZON LINUX
                  ↓
                USER
                  ↓
             PERMISSIONS
                  ↓
              PROCESS
                  ↓
              SERVICE
                  ↓
                PORT
                  ↓
              NETWORK
                  ↓
            APPLICATION
                  ↓
                LOGS
                  ↓
             MONITORING
                  ↓
             AUTOMATION
                  ↓
           TROUBLESHOOTING
```

And for AWS specifically:

```text
EC2
 ↓
Amazon Linux
 ↓
SSH
 ↓
Users
 ↓
Permissions
 ↓
Packages
 ↓
Processes
 ↓
systemd Services
 ↓
Networking
 ↓
Firewall
 ↓
EBS Storage
 ↓
Nginx
 ↓
Application
 ↓
Logs
 ↓
Bash
 ↓
Cron
 ↓
Monitoring
 ↓
Automation
```

# ✅ AMAZON LINUX COURSE STATUS

| Lesson | Topic | Status |
|---|---|---|
| 01 | Linux & Amazon Linux Fundamentals | ✅ |
| 02 | Linux Architecture | ✅ |
| 03 | Filesystem | ✅ |
| 04 | Essential Commands | ✅ |
| 05 | Files & Directories | ✅ |
| 06 | Permissions | ✅ |
| 07 | Users & Groups | ✅ |
| 08 | YUM & RPM | ✅ |
| 09 | Processes & Services | ✅ |
| 10 | Networking Basics | ✅ |
| 11 | Advanced Networking | ✅ |
| 12 | Bash Fundamentals | ✅ |
| 13 | Logs & Text Processing | ✅ |
| 14 | Troubleshooting | ✅ |
| 15 | Security | ✅ |
| 16 | EBS & Storage | ✅ |
| 17 | Performance Monitoring | ✅ |
| 18 | Cron & Scheduling | ✅ |
| 19 | Environment Variables | ✅ |
| 20 | Advanced Bash | ✅ |
| 21 | SSH | ✅ |
| 22 | Network Troubleshooting | ✅ |
| 23 | Nginx/Web Server | ✅ |
| 24 | Automation & DevOps Administration | ✅ |
| 25 | Final AWS Amazon Linux Project | ✅ |

## 🏆 Final goal

After these 25 lessons, your Linux knowledge should not just be **“I know Linux commands.”**

It should be:

> **“I can administer, troubleshoot, secure, monitor and automate an Amazon Linux server running on AWS EC2.”**
