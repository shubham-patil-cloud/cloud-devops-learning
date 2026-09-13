# 🐧 Amazon Linux / Linux Interview Questions
## Basic → Intermediate → Advanced → Scenario-Based

Below is a **complete interview question bank** aligned with the 25 Amazon Linux lessons and your target roles: **AWS Cloud Engineer, Cloud Support Engineer, Junior DevOps Engineer**.

---

# 🟢 LEVEL 1 — BASIC LINUX QUESTIONS

## Linux Fundamentals

### 1. What is Linux?
Linux is an open-source operating-system kernel used by many operating systems and server distributions.

### 2. What is Amazon Linux?
Amazon Linux is an AWS-provided Linux distribution optimized for AWS workloads, commonly used on EC2.

### 3. What is a Linux distribution?
A Linux distribution combines the Linux kernel with system utilities, libraries, package management, and applications.

Examples:
- Amazon Linux
- Ubuntu
- RHEL
- Debian

### 4. What is the Linux kernel?
The kernel manages CPU, memory, processes, devices, filesystems, and networking.

### 5. What is Bash?
Bash is a command-line shell and scripting environment.

### 6. What is a shell?
A shell provides an interface for users to interact with the operating system.

### 7. What is CLI?
CLI means **Command Line Interface**.

### 8. What is the difference between CLI and GUI?
CLI uses commands, while GUI uses graphical interfaces.

### 9. Why is Linux widely used in DevOps?
Because Linux is widely used for servers and cloud workloads and provides powerful command-line tools, automation, networking, security, and scripting capabilities.

### 10. What is the difference between Linux and Amazon Linux?
Linux is the kernel; Amazon Linux is a complete Linux distribution provided by AWS.

---

# 🟢 BASIC COMMAND QUESTIONS

### 11. How do you check the current user?

```bash
whoami
```

### 12. How do you check the OS version?

```bash
cat /etc/os-release
```

### 13. How do you check the kernel version?

```bash
uname -r
```

### 14. How do you check the hostname?

```bash
hostname
```

### 15. How do you check system uptime?

```bash
uptime
```

### 16. How do you find the current directory?

```bash
pwd
```

### 17. How do you list files?

```bash
ls
```

### 18. How do you show hidden files?

```bash
ls -la
```

### 19. How do you create a directory?

```bash
mkdir test
```

### 20. How do you create nested directories?

```bash
mkdir -p app/logs
```

### 21. How do you create an empty file?

```bash
touch file.txt
```

### 22. How do you copy a file?

```bash
cp source.txt destination.txt
```

### 23. How do you copy a directory?

```bash
cp -r source destination
```

### 24. How do you rename a file?

```bash
mv old.txt new.txt
```

### 25. How do you delete a file?

```bash
rm file.txt
```

### 26. How do you delete a directory recursively?

```bash
rm -r directory
```

### 27. What is `rm -rf`?
It recursively and forcefully removes files/directories.

It is dangerous because mistakes can cause destructive deletion.

---

# 🟢 FILESYSTEM QUESTIONS

### 28. What is `/`?

It is the root of the Linux filesystem hierarchy.

### 29. What is `/home`?

Contains home directories of normal users.

### 30. What is `/root`?

The home directory of the root user.

### 31. What is `/etc`?

Contains system and application configuration files.

### 32. What is `/var`?

Contains variable data such as logs, caches and application data.

### 33. What is `/var/log`?

Contains system and application logs.

### 34. What is `/tmp`?

Used for temporary files.

### 35. What is `/dev`?

Contains device files.

### 36. What is `/proc`?

A virtual filesystem exposing process and kernel information.

### 37. What is `/sys`?

Provides information and interfaces related to devices and the kernel.

### 38. What is the difference between `/root` and `/`?

`/` is the filesystem root; `/root` is the root user's home directory.

### 39. What is an absolute path?

A path beginning from `/`.

Example:

```bash
/etc/nginx/nginx.conf
```

### 40. What is a relative path?

A path relative to the current directory.

---

# 🟢 FILE MANAGEMENT QUESTIONS

### 41. How do you find a file?

```bash
find /home -name "file.txt"
```

### 42. How do you find all `.log` files?

```bash
find /var/log -type f -name "*.log"
```

### 43. How do you find files larger than 500 MB?

```bash
find /var -type f -size +500M
```

### 44. What does `.` mean?

Current directory.

### 45. What does `..` mean?

Parent directory.

### 46. What does `~` mean?

Current user's home directory.

### 47. What is a symbolic link?

A symbolic link is a filesystem object that points to another path.

```bash
ln -s /var/log/nginx/access.log access.log
```

### 48. What is a hard link?

A hard link is another directory entry referring to the same inode.

---

# 🟡 LEVEL 2 — INTERMEDIATE

# Permissions

### 49. What are Linux file permissions?

Linux permissions control read, write and execute access.

```text
r = read
w = write
x = execute
```

### 50. What are the three permission categories?

```text
User
Group
Others
```

### 51. Explain `755`.

```text
rwxr-xr-x
```

Owner:

```text
rwx = 7
```

Group:

```text
r-x = 5
```

Others:

```text
r-x = 5
```

### 52. Explain `644`.

```text
rw-r--r--
```

### 53. What does `chmod` do?

Changes file or directory permissions.

```bash
chmod 755 script.sh
```

### 54. What does `chown` do?

Changes ownership.

```bash
chown ec2-user file
```

### 55. What does `chgrp` do?

Changes group ownership.

### 56. Why shouldn't you use `chmod 777` unnecessarily?

Because it gives everyone read, write and execute permissions and can create security risks.

### 57. What permissions are commonly used for SSH private keys?

Often:

```bash
chmod 400 key.pem
```

or appropriately restrictive permissions such as `600`.

---

# Users & Groups

### 58. How do you create a user?

```bash
sudo useradd -m devops
```

### 59. How do you create a group?

```bash
sudo groupadd developers
```

### 60. How do you add a user to a group?

```bash
sudo usermod -aG developers devops
```

### 61. Why is `-a` important?

It appends the group without removing the user's existing supplementary groups.

### 62. What is UID?

User ID.

### 63. What is GID?

Group ID.

### 64. What is the difference between primary and supplementary groups?

The primary group is the user's default group; supplementary groups provide additional group membership.

### 65. What is `/etc/passwd`?

Contains user account information.

### 66. What is `/etc/shadow`?

Contains protected password-related account information.

### 67. What is the `wheel` group in Amazon Linux?

It is commonly used to provide sudo privileges to administrators.

### 68. How do you check a user's groups?

```bash
id username
groups username
```

---

# 🟡 PACKAGE MANAGEMENT

### 69. What package manager do you primarily use in this Amazon Linux curriculum?

```bash
yum
```

### 70. How do you install Nginx?

```bash
sudo yum install nginx -y
```

### 71. How do you update packages?

```bash
sudo yum update -y
```

### 72. How do you remove a package?

```bash
sudo yum remove nginx
```

### 73. How do you search for a package?

```bash
yum search nginx
```

### 74. What is RPM?

RPM is the package format and lower-level package-management system used by RPM-based distributions.

### 75. YUM vs RPM?

> YUM works with repositories and handles dependency resolution, while RPM works directly with RPM packages and provides lower-level package operations.

### 76. How do you check whether Nginx is installed?

```bash
rpm -q nginx
```

---

# 🟡 PROCESSES

### 77. What is a process?

A running instance of a program.

### 78. What is PID?

Process ID.

### 79. How do you list processes?

```bash
ps aux
```

### 80. How do you monitor processes interactively?

```bash
top
```

### 81. How do you find a process by name?

```bash
pgrep -a nginx
```

### 82. How do you terminate a process?

```bash
kill PID
```

### 83. What is `kill -9`?

It sends SIGKILL, which forcefully terminates the process.

### 84. Why shouldn't `kill -9` always be your first choice?

SIGKILL doesn't allow the process to perform normal cleanup. Normally try graceful termination first.

---

# 🟡 SERVICES & SYSTEMD

### 85. What is systemd?

systemd is the system and service manager used by modern Linux distributions, including Amazon Linux.

### 86. How do you check Nginx?

```bash
systemctl status nginx
```

### 87. How do you start Nginx?

```bash
sudo systemctl start nginx
```

### 88. How do you stop Nginx?

```bash
sudo systemctl stop nginx
```

### 89. How do you restart Nginx?

```bash
sudo systemctl restart nginx
```

### 90. What is the difference between restart and reload?

**Restart** stops and starts the service.

**Reload** asks the service to reload configuration without a full stop/start when supported.

### 91. What does `enable` do?

Configures a service to start automatically during boot.

### 92. What does `enable --now` do?

Enables the service for boot and starts it immediately.

### 93. How do you see service logs?

```bash
journalctl -u nginx
```

### 94. How do you follow logs in real time?

```bash
journalctl -u nginx -f
```

---

# 🟡 LEVEL 2 — NETWORKING

### 95. What is an IP address?

An address used to identify a network interface/host at the IP layer.

### 96. What is a port?

A logical endpoint used by network applications.

### 97. What port does SSH use by default?

```text
22
```

### 98. HTTP?

```text
80
```

### 99. HTTPS?

```text
443
```

### 100. What is TCP?

A connection-oriented transport protocol providing reliable, ordered delivery.

### 101. What is UDP?

A connectionless transport protocol with lower overhead and no built-in delivery guarantee.

### 102. How do you check IP addresses?

```bash
ip a
```

### 103. How do you check routing?

```bash
ip route
```

### 104. How do you see listening ports?

```bash
ss -lntp
```

### 105. How do you test HTTP?

```bash
curl http://localhost
```

### 106. How do you test DNS?

```bash
dig example.com
```

### 107. How do you test a TCP port?

```bash
nc -zv SERVER 8080
```

---

# 🟠 LEVEL 3 — ADVANCED

# Bash & Automation

### 108. What is a Bash script?

A text file containing commands and shell logic that Bash executes.

### 109. Why use `#!/bin/bash`?

It specifies Bash as the interpreter.

### 110. What is `$?`?

Exit status of the most recently executed command.

### 111. What does exit code `0` normally mean?

Success.

### 112. What are `$0`, `$1`, `$2`?

```text
$0 = script name
$1 = first argument
$2 = second argument
```

### 113. What is `$#`?

Number of positional arguments.

### 114. What is `$@`?

The positional arguments, preserving argument boundaries when quoted correctly.

### 115. What does `set -euo pipefail` do?

Provides stricter Bash error handling:

- `-e` → exit on unhandled command failure
- `-u` → treat unset variables as errors
- `pipefail` → pipeline fails if an element fails

### 116. How do you debug a Bash script?

```bash
bash -x script.sh
```

### 117. How do you syntax-check a Bash script?

```bash
bash -n script.sh
```

### 118. What is `trap`?

It allows a script to execute specified actions when receiving signals or certain shell events.

---

# 🟠 LOGS & TEXT PROCESSING

### 119. How do you search logs for errors?

```bash
grep "ERROR" app.log
```

### 120. How do you count errors?

```bash
grep -c "ERROR" app.log
```

### 121. How do you monitor a log in real time?

```bash
tail -f /var/log/nginx/access.log
```

### 122. What is AWK?

A text-processing language useful for field extraction, filtering and reporting.

### 123. Find the first field:

```bash
awk '{print $1}' access.log
```

### 124. How do you count unique IPs?

```bash
awk '{print $1}' access.log | sort | uniq -c
```

### 125. How do you find the most frequent IP?

```bash
awk '{print $1}' access.log | sort | uniq -c | sort -nr
```

### 126. What does `sed` do?

It performs stream-based text transformations such as substitution and deletion.

### 127. What does `cut` do?

Extracts selected fields/characters from input.

---

# 🟠 STORAGE

### 128. What is EBS?

Amazon Elastic Block Store provides persistent block storage for EC2.

### 129. What is a block device?

A device that provides block-oriented storage access.

### 130. How do you see attached disks?

```bash
lsblk
```

### 131. How do you see filesystem usage?

```bash
df -h
```

### 132. `lsblk` vs `df -h`?

> `lsblk` shows block devices and their structure; `df -h` shows filesystem capacity and usage.

### 133. What is a partition?

A logical subdivision of a disk/device.

### 134. What is a filesystem?

A structure used to organize and store files on a storage device.

### 135. What filesystem is commonly used in your Amazon Linux labs?

XFS.

### 136. How do you create an XFS filesystem?

```bash
sudo mkfs.xfs /dev/xvdf1
```

### 137. What is mounting?

Attaching a filesystem to a directory in the Linux filesystem hierarchy.

### 138. How do you mount a filesystem?

```bash
sudo mount /dev/xvdf1 /data
```

### 139. What is `/etc/fstab`?

Configuration file describing filesystems that should be mounted automatically.

### 140. Why use UUID in `/etc/fstab`?

UUID provides a stable filesystem identifier that is generally more reliable than depending on potentially changing device names.

---

# 🔴 ADVANCED STORAGE SCENARIO

### 141. You increased an EBS volume from 10 GB to 20 GB, but Linux still shows 10 GB. What do you do?

Strong interview answer:

> First I verify the EBS volume size and then check `lsblk`. If the partition hasn't grown, I grow the partition using the appropriate tool such as `growpart`. Then I grow the filesystem using the filesystem-specific command, such as `xfs_growfs` for XFS. Finally I verify with `df -h`.

Commands may include:

```bash
lsblk
df -h
sudo growpart /dev/xvdf 1
sudo xfs_growfs /data
df -h
```

---

# 🔴 SECURITY QUESTIONS

### 142. Authentication vs authorization?

> Authentication verifies who you are; authorization determines what you are allowed to access.

### 143. What is least privilege?

Give users/services only the permissions they actually need.

### 144. What is SELinux?

Security-Enhanced Linux provides mandatory access control mechanisms.

### 145. How do you check SELinux mode?

```bash
getenforce
```

### 146. What is firewalld?

A firewall management service commonly used on RPM-based Linux systems.

### 147. How do you check it?

```bash
sudo systemctl status firewalld
```

### 148. Security Group vs Linux firewall?

> An AWS Security Group controls traffic at the AWS network-interface level, while the Linux firewall controls traffic at the operating-system level.

### 149. Why should SSH private keys be protected?

Because anyone who obtains an authorized private key may potentially authenticate to the associated server.

### 150. Where is SSH server configuration?

```text
/etc/ssh/sshd_config
```

### 151. How do you validate SSH configuration?

```bash
sudo sshd -t
```

---

# 🔴 SSH SCENARIO QUESTIONS

### 152. You cannot SSH into an EC2 instance. How do you troubleshoot?

Answer in layers:

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

Then check:

```bash
systemctl status sshd
ss -lntp
journalctl -u sshd
```

Also verify:

- correct public IP
- correct username
- correct key
- key permissions
- Security Group
- route/network path
- instance state

---

# 🔴 PERFORMANCE QUESTIONS

### 153. How do you investigate high CPU?

```bash
uptime
top
ps aux --sort=-%cpu | head
```

Then identify the process and investigate why it is consuming CPU.

### 154. How do you investigate high memory usage?

```bash
free -h
ps aux --sort=-%mem | head
```

### 155. How do you investigate disk space?

```bash
df -h
du -sh /*
```

### 156. What is the difference between disk space and disk I/O?

> Disk space is how much storage capacity is consumed. Disk I/O measures how actively the storage device is reading and writing data.

### 157. How do you investigate disk I/O?

```bash
iostat -xz 2
```

### 158. What is load average?

It represents the amount of runnable/uninterruptible work over 1, 5 and 15-minute periods. Interpretation should consider CPU count and workload characteristics.

---

# 🔴 NGINX QUESTIONS

### 159. What is Nginx?

Nginx is a high-performance web server and reverse proxy.

### 160. How do you install Nginx on Amazon Linux?

```bash
sudo yum install nginx -y
```

### 161. How do you start it?

```bash
sudo systemctl enable --now nginx
```

### 162. Where is the main configuration?

```text
/etc/nginx/nginx.conf
```

### 163. Where are Nginx logs?

```text
/var/log/nginx/
```

### 164. How do you validate Nginx configuration?

```bash
sudo nginx -t
```

### 165. What is a reverse proxy?

A server that receives client requests and forwards them to backend servers/applications.

```text
Client
 ↓
Nginx
 ↓
Application
```

---

# 🔴 REAL DEVOPS SCENARIOS

## 166. Nginx is running but the website isn't accessible. What do you check?

```text
1. Nginx service
2. Listening port
3. Local curl
4. Nginx configuration
5. Linux firewall
6. AWS Security Group
7. Route/network
8. Logs
```

Commands:

```bash
systemctl status nginx
ss -lntp
curl http://localhost
nginx -t
journalctl -u nginx
```

---

## 167. Nginx returns 502. What do you check?

A 502 commonly indicates a problem communicating with the upstream backend.

Check:

```bash
curl http://127.0.0.1:8080
ss -lntp
ps aux
journalctl -u nginx
```

Then verify:

- backend is running
- correct backend port
- correct proxy configuration
- backend bind address
- application logs

---

## 168. Server disk is 100% full. What do you do?

```bash
df -h
df -i
du -sh /*
```

Then find large files:

```bash
find /var -type f -size +500M
```

Also check:

```bash
lsof +L1
```

for deleted files still held open by processes.

---

## 169. Application is running but users cannot connect to port 8080.

Troubleshooting:

```bash
ss -lntp
curl http://127.0.0.1:8080
```

Check whether it listens on:

```text
127.0.0.1:8080
```

or:

```text
0.0.0.0:8080
```

Then check:

```text
Linux firewall
Security Group
NACL
Route
```

---

## 170. Cron job works manually but not through cron. Why?

Possible reasons:

- different PATH
- different environment
- relative paths
- permissions
- wrong user
- working-directory assumptions
- missing environment variables
- SELinux/security restrictions
- script errors

Use absolute paths and logging:

```text
>> /path/to/job.log 2>&1
```

---

# 🔴 EXPERT-LEVEL AWS + LINUX QUESTIONS

### 171. Explain the complete path of an HTTP request to an EC2-hosted Nginx application.

```text
Internet
 ↓
Internet Gateway
 ↓
VPC Route
 ↓
Subnet
 ↓
Security Group
 ↓
EC2 ENI
 ↓
Linux Network Stack
 ↓
Linux Firewall
 ↓
Nginx :80
 ↓
Reverse Proxy
 ↓
Application :8080
```

---

### 172. EC2 is reachable by SSH but HTTP doesn't work. What does that tell you?

It suggests the instance and SSH path are functioning, but HTTP may have a problem involving:

- Nginx/application
- port 80 listener
- Linux firewall
- Security Group HTTP rule
- NACL
- routing
- local configuration

Don't assume the problem is the entire EC2 instance.

---

### 173. Nginx is active, port 80 is listening, but remote HTTP still fails. What next?

Check:

```text
curl localhost
```

If local access works, investigate outside Nginx:

```text
Linux firewall
 ↓
Security Group
 ↓
NACL
 ↓
Route table
 ↓
Subnet/VPC
```

---

### 174. Application works with localhost but not with the EC2 private IP. Why?

Check what address the application is bound to.

For example:

```text
127.0.0.1:8080
```

accepts local connections only.

A service bound appropriately to the host interface(s), such as:

```text
0.0.0.0:8080
```

can accept remote connections, subject to network/firewall controls.

---

### 175. What happens when you run `systemctl restart nginx`?

systemd asks the Nginx service to stop and start again.

You should verify:

```bash
systemctl status nginx
ss -lntp
curl http://localhost
```

---

### 176. What happens if `/etc/fstab` contains a bad entry and you reboot?

The system may experience boot/mount problems depending on the failure and configuration. This is why you should test:

```bash
sudo mount -a
```

before rebooting and use options such as `nofail` where appropriate.

---

### 177. How would you troubleshoot an EC2 server that suddenly became slow?

Start broad:

```bash
uptime
top
free -h
df -h
iostat -xz 2
ss -s
```

Then:

```text
CPU
 ↓
Memory
 ↓
Disk
 ↓
Network
 ↓
Processes
 ↓
Services
 ↓
Logs
 ↓
Application
```

Then compare with AWS monitoring such as CloudWatch.

---

### 178. How would you investigate repeated Nginx 500 errors?

Start with logs:

```bash
grep "500" /var/log/nginx/access.log
grep -i "error" /var/log/nginx/error.log
```

Then determine whether the issue is:

```text
Nginx
 ↓
Reverse proxy
 ↓
Application
 ↓
Database
```

Check backend logs and application health.

---

### 179. How would you troubleshoot an EC2 instance that cannot access the Internet?

Linux:

```bash
ip a
ip route
ping 8.8.8.8
dig google.com
```

Then AWS:

```text
ENI
 ↓
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

For a private subnet, verify the appropriate NAT architecture.

---

### 180. How would you securely automate AWS operations from an EC2 server?

Prefer an appropriate **IAM role attached to the EC2 instance** rather than storing long-lived AWS access keys in scripts.

---

# 🔥 TOP 20 QUESTIONS YOU MUST BE ABLE TO ANSWER IN AN INTERVIEW

If you're short on time, prioritize these:

1. **What is Linux and why is it used in DevOps?**
2. **What is Amazon Linux?**
3. **Explain Linux filesystem structure.**
4. **Explain Linux permissions and `755`/`644`.**
5. **`chmod` vs `chown`?**
6. **How do users and groups work?**
7. **What is the `wheel` group?**
8. **YUM vs RPM?**
9. **Process vs service?**
10. **`kill` vs `systemctl stop`?**
11. **How do you troubleshoot a service that isn't running?**
12. **How do you check listening ports?**
13. **How do you troubleshoot an unreachable port?**
14. **How do you troubleshoot high CPU/memory/disk?**
15. **What is EBS and how do you mount it on Amazon Linux?**
16. **What happens when an EBS volume is expanded?**
17. **How does SSH to EC2 work?**
18. **Security Group vs Linux firewall?**
19. **How do you troubleshoot Nginx 502?**
20. **Explain your complete EC2 Linux troubleshooting approach.**

---

# 🏆 FINAL INTERVIEW MENTAL MODEL

When an interviewer gives you **any Linux/AWS server problem**, don't randomly run commands.

Think:

```text
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
IP
 ↓
ROUTE
 ↓
FIREWALL
 ↓
AWS SECURITY GROUP
 ↓
NACL
 ↓
APPLICATION
 ↓
LOGS
 ↓
MONITORING
 ↓
ROOT CAUSE
 ↓
FIX
 ↓
VERIFY
```

And your standard answer structure should be:

> **“First I will verify the symptom, then identify the affected layer, investigate logs and metrics, fix the root cause, verify the service, and finally monitor it to make sure the issue doesn't recur.”**

That is the interview style you should use for **Cloud Support / AWS / Junior DevOps** roles.
