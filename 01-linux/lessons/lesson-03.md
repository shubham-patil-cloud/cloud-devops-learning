# 🐧 Linux Fundamentals — Lesson 3
## Linux Filesystem & Directory Structure

This is one of the **most important Linux topics for AWS/DevOps**.

When you log into an EC2 Linux server, you'll constantly work with directories such as `/etc`, `/var`, `/home`, `/tmp`, and `/opt`.

---

# 1. What is a Filesystem?

A **filesystem** is the structure Linux uses to organize and store files and directories on storage devices.

Windows commonly uses:

```text
C:\
D:\
```

Linux uses **one unified directory tree**.

The top of this tree is:

```text
/
```

This is called the **root directory**.

⚠️ Don't confuse:

```text
/       → root directory
root    → root user's home directory/account
```

---

# 2. Linux Filesystem Structure

A simplified Linux filesystem looks like this:

```text
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── tmp
├── usr
└── var
```

You don't need to memorize every directory immediately.

For Cloud/DevOps, focus heavily on:

```text
/
├── /home
├── /root
├── /etc
├── /var
├── /tmp
├── /opt
├── /usr
├── /bin
├── /sbin
├── /dev
├── /proc
└── /boot
```

---

# 3. `/` — Root Directory ⭐⭐⭐

```text
/
```

The `/` directory is the **starting point of the entire Linux filesystem**.

Everything exists underneath `/`.

Example:

```text
/
├── home
├── etc
├── var
├── tmp
└── usr
```

You can see it using:

```bash
ls /
```

---

# 4. `/home` — User Home Directories ⭐⭐⭐

`/home` contains the home directories of normal users.

Example:

```text
/home
├── shubham
├── rahul
└── admin
```

If you have a user called `shubham`:

```text
/home/shubham
```

This is generally where that user's personal files are stored.

Example:

```bash
cd /home
ls
```

---

# 5. `/root` — Root User's Home Directory ⭐⭐⭐

`/root` is the home directory of the **root user**.

```text
/root
```

Important:

```text
/       = filesystem root
/root   = root user's home
```

These are completely different concepts.

---

# 6. `/etc` — Configuration Files ⭐⭐⭐⭐⭐

This is **extremely important for Cloud/DevOps**.

`/etc` contains system and application configuration files.

Examples:

```text
/etc/hosts
/etc/hostname
/etc/passwd
/etc/group
/etc/fstab
/etc/ssh/
/etc/nginx/
```

For example:

```bash
cat /etc/hosts
```

You might see:

```text
127.0.0.1   localhost
```

### DevOps importance

When troubleshooting servers, you'll frequently inspect configuration under:

```text
/etc
```

### Remember:

> **`/etc` = configuration**

---

# 7. `/var` — Variable Data ⭐⭐⭐⭐⭐

`/var` contains data that changes frequently.

Examples:

- Logs
- Application data
- Cache
- Spool files

Most importantly:

```text
/var/log
```

contains system/application logs.

Example:

```bash
ls /var/log
```

You may see files such as:

```text
messages
secure
cron
```

depending on the Linux distribution.

### Very important DevOps command

```bash
tail -f /var/log/some-log-file
```

This lets you watch a log file as new entries are added.

### Remember:

> **`/var` = variable data**

---

# 8. `/tmp` — Temporary Files ⭐⭐⭐

`/tmp` is used for temporary files.

Example:

```bash
cd /tmp
```

You can create a temporary file:

```bash
touch /tmp/test.txt
```

Applications often use `/tmp` for temporary data.

### Remember:

> **`/tmp` = temporary**

Don't use it as permanent application storage.

---

# 9. `/opt` — Optional/Third-Party Software

`/opt` is commonly used for **optional or third-party software**.

For example:

```text
/opt/myapplication
```

You may encounter application installations under `/opt`.

### DevOps use

You might deploy an application such as:

```text
/opt/myapp/
├── app
├── config
└── logs
```

---

# 10. `/usr` — User Programs and Data

`/usr` contains many user-space programs, libraries and shared data.

Common directories include:

```text
/usr/bin
/usr/sbin
/usr/lib
/usr/local
```

For example:

```bash
ls /usr/bin
```

You'll find many executable programs there.

---

# 11. `/bin` — Essential Commands

Traditionally `/bin` contains essential executable commands.

Examples include commands such as:

```text
ls
cp
mv
cat
```

On some modern Linux distributions, `/bin` may be a symbolic link to `/usr/bin`.

For learning purposes:

> **`/bin` = essential user commands**

---

# 12. `/sbin` — System Administration Commands

`/sbin` traditionally contains system administration executables.

These are primarily intended for administrative/system-management tasks.

On modern distributions, `/sbin` may also be linked into `/usr/sbin`.

Remember:

> **`/sbin` = system administration commands**

---

# 13. `/boot` — Boot Files

`/boot` contains files needed during the Linux boot process.

Examples can include:

- Linux kernel
- Bootloader-related files
- Initial RAM filesystem

You normally don't modify `/boot` during everyday DevOps work.

---

# 14. `/dev` — Devices ⭐⭐⭐⭐

`/dev` contains special files representing devices.

Examples:

```text
/dev/sda
/dev/xvda
/dev/nvme0n1
```

These can represent disks/devices depending on the system and virtualization environment.

This becomes especially important when working with:

- AWS EBS
- Partitions
- Mounting disks
- Filesystems

For example:

```bash
lsblk
```

can show block devices and their relationships.

---

# 15. `/proc` — Process & Kernel Information ⭐⭐⭐⭐

`/proc` is a **virtual filesystem**.

It provides information about:

- Processes
- CPU
- Memory
- Kernel
- System information

Example:

```bash
ls /proc
```

You can inspect CPU information:

```bash
cat /proc/cpuinfo
```

Memory information:

```bash
cat /proc/meminfo
```

This is very useful during troubleshooting.

---

# 16. `/sys` — Kernel & Device Information

`/sys` is another virtual filesystem that exposes information about:

- Devices
- Drivers
- Kernel subsystems

You won't need to work deeply with it initially, but you should know what it is.

---

# 17. `/run` — Runtime Data

`/run` contains **runtime information** created after the system boots.

It can contain things such as:

- PID files
- Sockets
- Runtime service information

You generally don't store permanent application data here.

---

# 18. `/mnt` — Mount Point

`/mnt` is traditionally used as a temporary mount point for filesystems.

For example, you might mount a disk:

```bash
sudo mount /dev/xvdf1 /mnt
```

Then:

```bash
ls /mnt
```

can show the contents of that mounted filesystem.

---

# 19. `/media` — Removable Media

`/media` is commonly used for automatically mounted removable devices.

Examples:

- USB drives
- CDs/DVDs

Less important for your AWS/DevOps work.

---

# 20. The Directories You MUST Know

For your career, prioritize these:

| Directory | Purpose | Priority |
|---|---|---:|
| `/` | Root of filesystem | ⭐⭐⭐⭐⭐ |
| `/etc` | Configuration | ⭐⭐⭐⭐⭐ |
| `/var/log` | Logs | ⭐⭐⭐⭐⭐ |
| `/home` | Normal users' home | ⭐⭐⭐⭐ |
| `/root` | Root user's home | ⭐⭐⭐⭐ |
| `/tmp` | Temporary files | ⭐⭐⭐⭐ |
| `/opt` | Optional/third-party software | ⭐⭐⭐ |
| `/usr` | Programs/libraries/data | ⭐⭐⭐ |
| `/dev` | Device files | ⭐⭐⭐⭐ |
| `/proc` | Process/kernel information | ⭐⭐⭐⭐ |
| `/boot` | Boot files | ⭐⭐ |
| `/mnt` | Mount point | ⭐⭐⭐ |
| `/run` | Runtime data | ⭐⭐ |
| `/sys` | Kernel/device information | ⭐⭐ |
| `/media` | Removable media | ⭐ |

---

# 21. Absolute Path vs Relative Path ⭐⭐⭐⭐⭐

This is very important.

## Absolute Path

Starts from `/`.

Example:

```text
/etc/hosts
```

```text
/var/log
```

```text
/home/shubham
```

The path gives the complete location.

---

## Relative Path

Starts from your **current directory**.

Suppose you're here:

```text
/home/shubham
```

And there is a directory:

```text
/home/shubham/projects
```

You can use:

```bash
cd projects
```

instead of:

```bash
cd /home/shubham/projects
```

---

# 22. `.` and `..`

Two special path references:

### `.`

Means:

> Current directory

Example:

```bash
ls .
```

### `..`

Means:

> Parent directory

Example:

```bash
cd ..
```

Suppose you're here:

```text
/home/shubham/projects
```

Running:

```bash
cd ..
```

takes you to:

```text
/home/shubham
```

---

# 23. `~` — Home Directory

The `~` symbol represents the current user's home directory.

If you're logged in as:

```text
ec2-user
```

then:

```bash
cd ~
```

takes you to that user's home directory.

You can check:

```bash
pwd
```

---

# 24. Important Practical Commands

### See current directory

```bash
pwd
```

### List files

```bash
ls
```

### List everything including hidden files

```bash
ls -la
```

### Go to root

```bash
cd /
```

### Go to `/etc`

```bash
cd /etc
```

### Go to `/var/log`

```bash
cd /var/log
```

### Go back

```bash
cd ..
```

### Go to home

```bash
cd ~
```

---

# 🧪 Mini Practical Lab

If you're using Amazon Linux or Ubuntu, try these one by one:

### Step 1

```bash
pwd
```

### Step 2

```bash
ls /
```

### Step 3

```bash
cd /etc
```

### Step 4

```bash
pwd
```

Expected:

```text
/etc
```

### Step 5

```bash
ls
```

### Step 6

```bash
cd /var/log
```

### Step 7

```bash
pwd
```

### Step 8

```bash
ls
```

### Step 9

```bash
cd ..
```

### Step 10

```bash
pwd
```

You should now be in:

```text
/var
```

---

# 🎤 Interview Questions

### Q1. What is `/` in Linux?

> `/` is the root directory and the starting point of the Linux filesystem hierarchy.

### Q2. What is `/etc` used for?

> `/etc` contains system and application configuration files.

### Q3. What is `/var` used for?

> `/var` stores variable data such as logs, caches, and other data that changes during system operation.

### Q4. Where are Linux logs generally stored?

> Many Linux system and application logs are stored under `/var/log`.

### Q5. What is `/home`?

> `/home` contains the home directories of normal users.

### Q6. What is `/root`?

> `/root` is the home directory of the root user.

### Q7. What is `/tmp`?

> `/tmp` is used for temporary files and data.

### Q8. What is `/proc`?

> `/proc` is a virtual filesystem that provides information about processes, memory, CPU, and the Linux kernel.

### Q9. What is the difference between `/` and `/root`?

> `/` is the root of the entire filesystem, while `/root` is the home directory of the root user.

### Q10. What is an absolute path?

> An absolute path specifies the complete location of a file or directory starting from `/`.

Example:

```text
/etc/ssh/sshd_config
```

---

# 🧠 Easy Memory Trick

Remember:

```text
/       → Everything starts here
/etc    → Configuration
/var    → Variable data / Logs
/home   → Users
/root   → Root user's home
/tmp    → Temporary
/opt    → Optional software
/usr    → Programs & libraries
/dev    → Devices
/proc   → Processes / system information
/boot   → Boot files
/mnt    → Mount
/run    → Runtime
/sys    → System/kernel information
```

### ⭐ Most important for your AWS/DevOps career:

```text
/etc
/var/log
/home
/root
/tmp
/dev
/proc
/opt
```

---

## ✅ Lesson 3 Checklist

- [x] What is a filesystem?
- [x] Root `/`
- [x] `/home`
- [x] `/root`
- [x] `/etc`
- [x] `/var`
- [x] `/tmp`
- [x] `/opt`
- [x] `/usr`
- [x] `/bin`
- [x] `/sbin`
- [x] `/boot`
- [x] `/dev`
- [x] `/proc`
- [x] `/sys`
- [x] `/run`
- [x] `/mnt`
- [x] `/media`
- [x] Absolute path
- [x] Relative path
- [x] `.`
- [x] `..`
- [x] `~`
- [x] Basic navigation commands
- [x] AWS/DevOps relevance
- [x] Interview questions

