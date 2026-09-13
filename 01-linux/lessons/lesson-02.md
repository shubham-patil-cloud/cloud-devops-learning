# 🐧 Linux Fundamentals — Lesson 2
## Linux Architecture

In Lesson 1, you learned that **Linux is a kernel** and that distributions such as **Amazon Linux and Ubuntu** provide a complete operating system around that kernel.

Now let's understand **how Linux is structured and how a command travels through the system**.

---

# 1. What is Linux Architecture?

Linux architecture describes the different layers/components of a Linux operating system and how they interact.

The basic structure is:

```text
┌───────────────────────────────┐
│          Applications         │
│   Nginx, Git, Docker, etc.    │
├───────────────────────────────┤
│      Shell / System Tools     │
│       Bash, commands, etc.    │
├───────────────────────────────┤
│        System Libraries       │
│   glibc and other libraries   │
├───────────────────────────────┤
│            Kernel             │
│ Process | Memory | Network    │
│ Filesystem | Devices | etc.   │
├───────────────────────────────┤
│           Hardware            │
│ CPU | RAM | Disk | Network    │
└───────────────────────────────┘
```

The most important layers for you are:

**Hardware → Kernel → Libraries → Shell → Applications**

---

# 2. Hardware

At the bottom is the physical hardware.

Examples:

- CPU
- RAM
- Hard disk / SSD
- Network card
- Keyboard
- Display
- USB devices

Linux needs to communicate with all of these.

But applications don't normally communicate directly with hardware.

That's where the **kernel** comes in.

---

# 3. Kernel ⭐⭐⭐

The **kernel is the heart of Linux**.

It sits between applications/software and hardware.

```text
Applications
      ↓
    Kernel
      ↓
   Hardware
```

The kernel manages system resources.

### Major responsibilities

### ① Process Management

Controls running programs.

For example:

```bash
ps
```

shows running processes.

---

### ② Memory Management

Controls RAM usage.

For example:

```bash
free -h
```

shows memory information.

---

### ③ File Management

Handles files and filesystems.

For example:

```bash
ls
```

asks Linux to list directory contents.

---

### ④ Device Management

Controls hardware devices through device drivers.

Examples:

- Disk
- Network interface
- USB device

---

### ⑤ Network Management

Handles network communication.

For example:

```bash
ip addr
```

shows network interfaces and IP addresses.

---

### ⑥ Security & Permissions

Controls who can access files and resources.

Example:

```bash
chmod 755 script.sh
```

---

# 4. System Libraries

System libraries provide functions that applications can use to communicate with the kernel.

You don't normally interact with them directly.

Think of them as a **bridge between applications and the kernel**.

```text
Application
     ↓
System Library
     ↓
Kernel
     ↓
Hardware
```

On Linux, **glibc** is one of the most important system libraries.

---

# 5. Shell

The shell provides a way for you to interact with Linux.

For example:

```bash
ls
```

```bash
cd /var/log
```

```bash
mkdir test
```

The shell interprets the commands and requests the required operations from the operating system.

### Common shells

- Bash
- Zsh
- Fish
- sh
- Ksh

For your Cloud/DevOps learning:

> **Bash is the most important shell to learn.**

---

# 6. System Utilities

Linux provides many commands/utilities for administration.

Examples:

```bash
ls
cp
mv
rm
grep
find
ps
top
df
du
chmod
chown
systemctl
```

These utilities allow administrators and DevOps engineers to manage Linux systems.

---

# 7. Applications

At the top are applications.

Examples:

- Nginx
- Apache
- MySQL
- Git
- Docker
- Jenkins
- Python
- Node.js

For example, when you install Nginx:

```bash
sudo yum install nginx -y
```

Nginx becomes an application/service running on your Linux system.

---

# 8. How Does a Command Work?

This is one of the most useful concepts to understand.

Suppose you execute:

```bash
ls
```

What happens?

```text
You type:
ls
 ↓
Terminal
 ↓
Bash Shell
 ↓
ls command
 ↓
System Libraries / System Calls
 ↓
Linux Kernel
 ↓
Filesystem / Storage
 ↓
Kernel returns result
 ↓
Bash
 ↓
Terminal displays result
```

You don't need to memorize every internal detail yet.

Just remember:

> **User → Shell → Kernel → Hardware/Resources → Result**

---

# 9. What is a System Call?

A **system call** is a mechanism through which a user-space program requests a service from the Linux kernel.

For example, an application might need to:

- Read a file
- Write to a file
- Create a process
- Allocate memory
- Communicate over a network

It requests the kernel through system calls.

Conceptually:

```text
Application
     ↓
System Call
     ↓
Kernel
     ↓
Hardware / Resource
```

You don't need to memorize system calls at this stage.

Later, when we cover **processes and troubleshooting**, this concept will become more useful.

---

# 10. User Space vs Kernel Space ⭐⭐⭐

This is an important interview concept.

Linux divides execution into two broad areas:

### User Space

Where normal applications run.

Examples:

```text
Nginx
Git
Python
Bash
Docker
```

### Kernel Space

Where the Linux kernel operates and has privileged access to system resources.

```text
User Space
──────────────
Applications
Shell
Utilities
──────────────
Kernel Space
──────────────
Linux Kernel
──────────────
Hardware
```

### Why separate them?

For **security and stability**.

A normal application should not have unrestricted access to hardware or critical kernel memory.

---

# 11. Root User

Linux has a special administrative account called:

```text
root
```

Root has extensive privileges over the system.

Example:

```bash
sudo yum install nginx -y
```

Here:

```text
sudo
 ↓
Execute command with elevated privileges
```

We'll study **users, groups, sudo and permissions** in detail later.

---

# 12. Linux Architecture — AWS Example ⭐

Imagine you launch an Amazon Linux EC2 instance.

```text
                 EC2 Instance
                      │
              ┌───────┴───────┐
              │               │
         Applications       Bash
         Nginx / App          │
              │               │
              └───────┬───────┘
                      ↓
                    Kernel
              ┌───────┼───────┐
              ↓       ↓       ↓
             CPU     RAM    Network
              │       │       │
              └───────┼───────┘
                      ↓
                 AWS Hardware
```

When you're working as a Cloud Engineer, you'll frequently interact with the **user-space side**, while the Linux kernel manages the underlying resources.

---

# 13. Important Difference: Kernel vs Shell

Don't confuse these in interviews.

| Kernel | Shell |
|---|---|
| Core of Linux | Interface to interact with OS |
| Manages hardware/resources | Interprets commands |
| Runs in kernel space | Runs in user space |
| Manages processes, memory, devices | Executes commands/scripts |
| Example: Linux kernel | Example: Bash |

### Easy memory trick

> **Kernel = Manager**  
> **Shell = Translator**

The kernel manages the system.

The shell translates your commands into actions.

---

# 14. Interview Questions 🎤

### Q1. What is Linux architecture?

**Answer:**

> Linux architecture consists of several layers including hardware, kernel, system libraries, shell/system utilities, and applications. The kernel acts as the core layer between software and hardware.

---

### Q2. What is the role of the Linux kernel?

> The Linux kernel manages system resources such as CPU, memory, processes, filesystems, devices, and networking, and provides an interface between applications and hardware.

---

### Q3. What is a shell?

> A shell is a command interpreter that allows users to interact with the Linux operating system by executing commands and scripts.

---

### Q4. What is Bash?

> Bash, or Bourne Again Shell, is one of the most commonly used command-line shells in Linux.

---

### Q5. What is the difference between user space and kernel space?

> User space is where normal applications and utilities execute, while kernel space is where the Linux kernel executes with privileged access to system resources.

---

### Q6. What is a system call?

> A system call is a mechanism that allows a user-space application to request services from the Linux kernel.

---

# 🧠 Quick Revision

Remember this diagram:

```text
                 USER
                   ↓
             APPLICATIONS
          Nginx / Git / Python
                   ↓
             SHELL / TOOLS
              Bash / ls
                   ↓
            SYSTEM LIBRARIES
                   ↓
                KERNEL
       ┌───────────┼───────────┐
       ↓           ↓           ↓
     CPU          RAM        NETWORK
       └───────────┼───────────┘
                   ↓
                HARDWARE
```

### ⭐ One-line interview answer

> **Linux architecture consists of hardware, kernel, system libraries, shell/system utilities, and applications, with the kernel acting as the central interface between software and hardware.**

---

## ✅ Lesson 2 Checklist

- [x] Linux architecture
- [x] Hardware
- [x] Kernel
- [x] Kernel responsibilities
- [x] System libraries
- [x] Shell
- [x] Bash
- [x] System utilities
- [x] Applications
- [x] System calls
- [x] User space
- [x] Kernel space
- [x] Root user
- [x] AWS/EC2 architecture example
- [x] Interview questions
