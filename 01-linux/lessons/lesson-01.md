# 🐧 Linux Fundamentals — Lesson 1


---

## 1. What is Linux?

**Linux is an open-source operating system kernel.**

In simple words:

> **Linux is the core software that manages the computer's hardware and allows applications to run.**

For example, when you run:

```bash
ls
```

Linux handles the request and interacts with the filesystem to show you files and directories.

---

## 2. What is an Operating System?

An **Operating System (OS)** acts as a bridge between:

**User / Applications ↔ Operating System ↔ Hardware**

Examples of operating systems:

- Windows
- Linux
- macOS
- Android
- iOS

The OS manages things like:

- CPU
- RAM
- Storage
- Network
- Files
- Processes
- Users
- Security

### Example

When you open a web browser:

```text
You
 ↓
Browser
 ↓
Operating System
 ↓
CPU / RAM / Storage / Network
```

---

# 3. What exactly is the Linux Kernel?

This is an important distinction.

**Linux itself technically refers to the kernel.**

The **kernel** is the core component responsible for communicating with hardware and managing system resources.

```text
                 USER
                   ↓
             Applications
                   ↓
                Shell
                   ↓
               KERNEL
        ┌──────────┼──────────┐
        ↓          ↓          ↓
       CPU        RAM       Storage
        ↓          ↓          ↓
              HARDWARE
```

### Kernel responsibilities

| Function | What it does |
|---|---|
| Process Management | Manages running programs |
| Memory Management | Manages RAM |
| File Management | Handles files/storage |
| Device Management | Communicates with hardware |
| Networking | Handles network communication |
| Security | Controls access and permissions |

---

# 4. Linux vs Linux Distribution

This is **very important for interviews**.

### Linux

Linux = **Kernel**

### Linux Distribution

A Linux distribution combines the Linux kernel with other software to create a complete operating system.

Examples:

- Ubuntu
- Amazon Linux
- Red Hat Enterprise Linux (RHEL)
- Rocky Linux
- Debian
- Fedora
- Kali Linux

Think of it like:

```text
Linux Kernel
     +
System Utilities
     +
Package Manager
     +
Libraries
     +
Applications
     ↓
Linux Distribution
```

---

# 5. What is a Linux Distribution?

A **Linux distribution (distro)** is a complete operating system built around the Linux kernel.

For your Cloud/DevOps career, focus mainly on:

### 🟢 Amazon Linux
Very important for AWS.

### 🟢 Ubuntu
Very common in cloud and DevOps environments.

### 🟢 RHEL
Very important for enterprise Linux.

You don't need to master every Linux distribution.

---
# 5. Linux vs Windows

| Feature	| Linux	| Windows |
|---|---|---|
| Source	| Open source	| Proprietary |
| CLI	| Very powerful	| PowerShell / CMD |
| Server usage	| Very common	| Common |
| Customization	| High	| More limited |
| Security	| Strong	| Strong |
| DevOps usage	| Very high	| Lower |
| Cloud usage	| Very common	| Common |

Linux is particularly important for Cloud and DevOps because of its strong command-line tools, automation capabilities, and widespread server usage.

---

# 6. Amazon Linux vs Ubuntu

You'll use both during your learning.

| Feature | Amazon Linux | Ubuntu |
|---|---|---|
| Common AWS usage | ⭐⭐⭐ | ⭐⭐⭐ |
| Package manager | `yum` | `apt` |
| Default user on EC2 | `ec2-user` | `ubuntu` |
| Based on | AWS-maintained | Debian |
| Cloud usage | Very high | Very high |

### Package installation

Amazon Linux:

```bash
sudo yum install nginx -y
```

Ubuntu:

```bash
sudo apt install nginx -y
```

For your learning, I'll use **`yum` for Amazon Linux examples**.

---

# 7. What is the Shell?

The **shell** is a program that allows you to communicate with the operating system using commands.

Example:

```bash
ls
```

You type the command → Shell interprets it → Linux performs the operation.

Common shells:

- Bash
- Zsh
- Fish
- sh
- Ksh

### Bash

**Bash = Bourne Again Shell**

Bash is extremely common in Linux and DevOps.

You'll use Bash heavily for:

- Automation
- Scripts
- Deployment
- Server administration
- CI/CD

---

# 8. Terminal vs Shell

Don't confuse these.

### Terminal

The terminal is the interface/window where you enter commands.

### Shell

The shell interprets those commands.

```text
You
 ↓
Terminal
 ↓
Shell (Bash)
 ↓
Linux Kernel
 ↓
Hardware
```

---

# 9. CLI vs GUI

### GUI

**Graphical User Interface**

You interact using:

- Mouse
- Windows
- Icons
- Menus

Example:

Windows desktop.

### CLI

**Command Line Interface**

You interact using commands.

Example:

```bash
ls
cd /var/log
cat file.txt
```

Cloud/DevOps engineers use the **CLI extensively**, because Linux servers are commonly managed remotely without a graphical desktop.

---

# 10. Why Linux is Important for Cloud & DevOps?

This is the main reason we're learning Linux first.

A large amount of cloud infrastructure runs Linux.

For example:

```text
AWS
 ↓
EC2
 ↓
Linux Server
 ↓
Nginx
 ↓
Application
```

You'll need Linux to:

- Connect to EC2
- Install software
- Configure web servers
- Manage services
- Check CPU/RAM
- Manage files
- Configure permissions
- Analyze logs
- Troubleshoot servers
- Write automation scripts
- Run Docker
- Run Kubernetes
- Work with CI/CD

So:

> **Linux is the foundation underneath much of your AWS + DevOps learning.**

---

# 11. Real AWS Example

Suppose you launch an **Amazon Linux EC2 instance**.

You connect:

```bash
ssh -i key.pem ec2-user@YOUR_PUBLIC_IP
```

Now you're inside the Linux server.

You might run:

```bash
pwd
```

```bash
ls
```

```bash
free -h
```

```bash
df -h
```

```bash
systemctl status nginx
```

Eventually you'll be able to troubleshoot an EC2 server almost entirely from the command line.

---

# 12. Important Linux Terminology

Learn these terms now:

| Term | Meaning |
|---|---|
| Linux | Open-source kernel |
| Kernel | Core of the OS |
| Distribution | Complete OS built around Linux |
| Shell | Command interpreter |
| Bash | Popular Linux shell |
| Terminal | Interface for entering commands |
| CLI | Command Line Interface |
| GUI | Graphical User Interface |
| Root | Linux superuser |
| Process | Running program |
| Filesystem | Structure for storing/managing files |

---

# 🎯 Interview Questions

### Q1. What is Linux?

**Answer:**

> Linux is an open-source operating system kernel that manages hardware resources and provides services to applications.

### Q2. What is a Linux distribution?

> A Linux distribution is a complete operating system built using the Linux kernel along with system utilities, libraries, package management tools, and applications.

### Q3. What is a kernel?

> The kernel is the core component of an operating system that manages CPU, memory, processes, devices, storage, and other system resources.

### Q4. What is Bash?

> Bash is a command-line shell commonly used in Linux to interact with the operating system and execute commands and scripts.

### Q5. Why is Linux important in DevOps?

> Linux is widely used for servers and cloud infrastructure, and DevOps tools such as Docker, Kubernetes, Jenkins, Terraform, and many CI/CD systems commonly run on Linux environments.

---

# 🧠 Remember This

```text
Linux
  ↓
Kernel
  ↓
Manages hardware/resources
  ↓
Distribution
  ↓
Amazon Linux / Ubuntu / RHEL
  ↓
Shell
  ↓
Bash
  ↓
Commands
  ↓
Server Administration
  ↓
AWS + DevOps
```

## ✅ Lesson 1 Checklist

- [x] What is Linux?
- [x] What is an OS?
- [x] What is the Linux kernel?
- [x] Linux vs Distribution
- [x] What is a Linux distribution?
- [x] Amazon Linux
- [x] Ubuntu
- [x] What is Shell?
- [x] What is Bash?
- [x] Terminal vs Shell
- [x] CLI vs GUI
- [x] Why Linux matters for AWS/DevOps
- [x] Basic interview questions

