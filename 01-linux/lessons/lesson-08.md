# 🐧 Lesson 8 — Linux Package Management

Package management is how you **install, update, remove, search, and manage software on Linux**.

For Cloud/DevOps, this is essential because on servers you will frequently install:

- Nginx
- Apache
- Git
- Docker
- Python
- Java
- Node.js
- `vim`
- `wget`
- `curl`
- monitoring agents
- security tools

Since you prefer **`yum` for Amazon Linux**, we'll use `yum` in the main examples and show the Ubuntu equivalent.

---

# 1. What is a Package?

A **package** is a bundle containing software and the files needed to install it.

For example:

```text
nginx package
     ↓
Nginx web server
     ↓
configuration files
     ↓
libraries/dependencies
     ↓
service files
```

Instead of manually downloading and configuring every required file, a package manager handles much of this automatically.

---

# 2. What is a Package Manager?

A package manager helps you:

```text
Install
Update
Remove
Search
Inspect
Manage dependencies
```

Common package managers:

| Linux | Package Manager |
|---|---|
| Amazon Linux / RHEL family | `yum` |
| Ubuntu / Debian | `apt` |
| Fedora | `dnf` |
| Arch Linux | `pacman` |

For **your learning**:

```text
Amazon Linux → yum
Ubuntu → apt
```

---

# 3. What is a Repository?

A **repository** is a location containing software packages that your package manager can download.

Think:

```text
Your EC2 Server
      ↓
     yum
      ↓
Repository
      ↓
Package
      ↓
Install
```

For example:

```bash
sudo yum install nginx
```

`yum` searches configured repositories, finds the package and its dependencies, downloads them and installs them.

---

# 4. Why Use Package Managers?

Without a package manager, you might have to:

```text
Download software
↓
Find dependencies
↓
Download dependencies
↓
Configure files
↓
Install manually
↓
Configure service
↓
Track updates
```

With a package manager:

```bash
sudo yum install package
```

Much easier.

---

# 5. `yum` — Amazon Linux

The basic syntax:

```bash
sudo yum command package
```

For example:

```bash
sudo yum install git
```

---

# 6. Update Package Information

You can refresh package metadata with:

```bash
sudo yum makecache
```

This updates the local metadata/cache used by `yum`.

Depending on the Amazon Linux version and repository configuration, package-manager behavior can vary, but the underlying concept remains the same.

---

# 7. Install a Package

Example:

```bash
sudo yum install git
```

You may see:

```text
Is this ok [y/d/N]:
```

Enter:

```text
y
```

Then verify:

```bash
git --version
```

Example:

```text
git version 2.x.x
```

---

# 8. Install Multiple Packages

You can install several packages at once:

```bash
sudo yum install git wget curl vim
```

This is useful when preparing an EC2 server.

For example:

```bash
sudo yum install git wget curl
```

---

# 9. Search for a Package

Use:

```bash
yum search nginx
```

Example:

```bash
yum search git
```

This searches available package information.

---

# 10. Check Package Information

Use:

```bash
yum info nginx
```

This can show information such as:

- Package name
- Version
- Architecture
- Repository
- Size
- Description

---

# 11. List Installed Packages

Use:

```bash
yum list installed
```

This can produce a large amount of output.

To search within it:

```bash
yum list installed | grep nginx
```

---

# 12. Check Whether a Package is Installed

Example:

```bash
yum list installed | grep git
```

Another useful command:

```bash
rpm -q git
```

If installed, it will return the package/version information.

---

# 13. `rpm` vs `yum`

This is an important interview topic.

### `rpm`

Works directly with RPM packages.

Example:

```bash
rpm -q git
```

### `yum`

A higher-level package manager that handles repositories and dependencies.

Example:

```bash
sudo yum install git
```

Think:

```text
yum
 ↓
Repository + dependency management
 ↓
RPM packages
```

---

# 14. Install a Local RPM

If you already have an RPM package:

```bash
sudo rpm -ivh package.rpm
```

But for normal repository-based installation, prefer:

```bash
sudo yum install package-name
```

because dependency resolution is handled for you.

---

# 15. Remove a Package

Use:

```bash
sudo yum remove git
```

or:

```bash
sudo yum erase git
```

Then verify:

```bash
git --version
```

You should get something like:

```text
command not found
```

if Git is no longer installed and no other copy is available in `PATH`.

---

# 16. Update a Specific Package

Example:

```bash
sudo yum update git
```

This updates Git if a newer version is available from the configured repositories.

---

# 17. Update All Packages

Use:

```bash
sudo yum update
```

This checks for available updates and updates packages.

On a production server, **don't blindly update everything without considering application compatibility and change-management requirements**.

---

# 18. Check Available Updates

You can use:

```bash
yum check-update
```

This checks for packages that have available updates.

Note that the command's exit status can be non-zero when updates are available, so don't interpret a non-zero exit code as automatically meaning the command failed.

---

# 19. Clean Package Cache

Useful commands include:

```bash
sudo yum clean all
```

This cleans cached repository/package data.

You may use this when troubleshooting stale package metadata or cache-related issues.

---

# 20. Package Dependencies

Many applications depend on other packages.

For example:

```text
Application
    ↓
Library A
    ↓
Library B
    ↓
Library C
```

A package manager can automatically resolve many of these dependencies.

That's one of the biggest advantages of using `yum`/`apt`.

---

# 21. Ubuntu — `apt`

Now compare with Ubuntu.

### Install

Amazon Linux:

```bash
sudo yum install nginx
```

Ubuntu:

```bash
sudo apt install nginx
```

### Update package metadata

Ubuntu:

```bash
sudo apt update
```

### Upgrade installed packages

```bash
sudo apt upgrade
```

### Remove

```bash
sudo apt remove nginx
```

### Search

```bash
apt search nginx
```

### Package information

```bash
apt show nginx
```

---

# 22. Important Difference: `update` vs `upgrade`

On Ubuntu:

```bash
sudo apt update
```

does **not** normally upgrade your installed packages.

It refreshes the package lists.

Then:

```bash
sudo apt upgrade
```

actually upgrades installed packages.

Conceptually:

```text
apt update
     ↓
Refresh package information
     ↓
apt upgrade
     ↓
Install available upgrades
```

For `yum`, the commonly used operation:

```bash
sudo yum update
```

can directly update installed packages.

---

# 23. Amazon Linux vs Ubuntu Cheat Sheet

| Task | Amazon Linux | Ubuntu |
|---|---|---|
| Install | `yum install` | `apt install` |
| Remove | `yum remove` | `apt remove` |
| Update packages | `yum update` | `apt upgrade` |
| Refresh metadata | `yum makecache` | `apt update` |
| Search | `yum search` | `apt search` |
| Package info | `yum info` | `apt show` |
| Installed packages | `yum list installed` | `apt list --installed` |
| Package query | `rpm -q` | `dpkg -s` |

---

# 24. Practical Lab — Install Git

Let's do a simple server setup.

### Step 1 — Check Git

```bash
git --version
```

If not installed:

```bash
sudo yum install git
```

---

### Step 2 — Verify

```bash
git --version
```

---

### Step 3 — Find package information

```bash
yum info git
```

---

### Step 4 — Check installation

```bash
yum list installed | grep git
```

---

# 25. Practical Lab — Install Nginx

Install:

```bash
sudo yum install nginx
```

Verify:

```bash
nginx -v
```

Then check the package:

```bash
yum info nginx
```

Check:

```bash
yum list installed | grep nginx
```

---

# 26. Package Management vs Service Management

This distinction is **very important**.

Installing software:

```bash
sudo yum install nginx
```

doesn't mean you've necessarily started the service.

Service management is a separate topic handled primarily by **systemd/systemctl**.

For example:

```bash
sudo systemctl start nginx
```

Check:

```bash
sudo systemctl status nginx
```

So:

```text
yum
 ↓
Install software

systemctl
 ↓
Manage running service
```

We'll cover `systemctl` in much more detail in the **Linux Processes & Services** lessons.

---

# 27. Real DevOps Server Setup

Imagine you launch an Amazon Linux EC2 instance for a web application.

You might install:

```bash
sudo yum install git
sudo yum install nginx
sudo yum install curl
sudo yum install wget
```

Or together:

```bash
sudo yum install git nginx curl wget
```

Then:

```text
EC2
 │
 ├── Git
 ├── Nginx
 ├── curl
 └── wget
```

Then configure and start the services as needed.

This is a very common server-preparation workflow.

---

# 28. Package Installation Troubleshooting

Suppose:

```bash
sudo yum install nginx
```

fails.

Don't immediately start changing random configuration.

Check:

### 1. Network connectivity

```bash
ping -c 4 8.8.8.8
```

You can also test DNS:

```bash
getent hosts google.com
```

### 2. Repository configuration

```bash
yum repolist
```

### 3. Search package

```bash
yum search nginx
```

### 4. Package information

```bash
yum info nginx
```

### 5. Check installed version

```bash
rpm -q nginx
```

This creates a basic troubleshooting flow:

```text
Package installation failed
        ↓
Network?
        ↓
Repository available?
        ↓
Package available?
        ↓
Dependencies?
        ↓
Version/conflict?
```

---

# 29. `yum repolist`

Very useful command:

```bash
yum repolist
```

It displays enabled repositories.

Conceptually:

```text
Repository
    ↓
Available packages
    ↓
yum
    ↓
Install/update
```

If the required repository isn't available, the package may not be found.

---

# 30. What is an RPM?

RPM originally refers to **Red Hat Package Manager** and is also the package format used by many Red Hat-family distributions.

An RPM package typically has a name similar to:

```text
package-version-release.architecture.rpm
```

Example structure:

```text
nginx-1.x.x-1.x86_64.rpm
│     │     │   │
│     │     │   └── Architecture
│     │     └────── Release
│     └──────────── Version
└────────────────── Package
```

---

# 31. Package Architecture

You may see:

```text
x86_64
```

This means the package is built for 64-bit x86 systems.

Modern AWS EC2 can also use ARM-based processors such as Graviton, where you may encounter:

```text
aarch64
```

Therefore, package architecture matters.

---

# 32. `rpm -qa`

List all installed RPM packages:

```bash
rpm -qa
```

This can produce a very long list.

Search:

```bash
rpm -qa | grep nginx
```

---

# 33. Find Which Package Owns a File

Suppose you have a file:

```text
/usr/bin/curl
```

You can ask which installed RPM owns it:

```bash
rpm -qf /usr/bin/curl
```

This is useful when troubleshooting where a file came from.

---

# 34. List Files Installed by a Package

Example:

```bash
rpm -ql nginx
```

This can show files installed by the package.

This is useful when you want to understand:

```text
Where is the configuration?
Where are binaries?
Where are documentation files?
```

---

# 35. Package Management in Automation

This becomes very important later.

With Ansible, instead of manually running:

```bash
sudo yum install nginx
```

you can automate package installation.

Conceptually:

```text
Ansible
   ↓
Package module
   ↓
yum
   ↓
Nginx
```

Terraform can provision the infrastructure, while Ansible can configure the server.

Example future workflow:

```text
Terraform
   ↓
EC2
   ↓
Ansible
   ↓
yum install nginx
   ↓
Configure Nginx
   ↓
Start service
```

This is exactly why package management matters in your DevOps roadmap.

---

# 36. Cloud/DevOps Package Management Workflow

Memorize this:

```text
EC2 launched
     ↓
Connect using SSH
     ↓
Check OS
     ↓
Update package metadata/system if appropriate
     ↓
Install required packages
     ↓
Configure packages
     ↓
Start services
     ↓
Verify
```

Example:

```bash
cat /etc/os-release
```

Then Amazon Linux:

```bash
sudo yum update
sudo yum install git nginx
```

Then:

```bash
git --version
nginx -v
```

Then service management:

```bash
sudo systemctl status nginx
```

---

# 🎯 Interview Questions

### Q1. What is package management?

Package management is the process of installing, updating, removing, searching and maintaining software packages on a Linux system.

---

### Q2. What is `yum`?

`yum` is a package-management tool used on RPM-based Linux systems such as Amazon Linux and traditionally RHEL-family systems.

---

### Q3. What is `apt`?

`apt` is the package-management tool commonly used on Debian-based distributions such as Ubuntu.

---

### Q4. Difference between `yum` and `rpm`?

```text
yum
→ Higher-level package manager
→ Uses repositories
→ Handles dependencies

rpm
→ Works directly with RPM packages
→ Lower-level package/package-database operations
```

---

### Q5. How do you install Nginx on Amazon Linux?

```bash
sudo yum install nginx
```

---

### Q6. How do you remove a package?

```bash
sudo yum remove package-name
```

---

### Q7. How do you update packages?

```bash
sudo yum update
```

---

### Q8. How do you search for a package?

```bash
yum search package-name
```

---

### Q9. How do you check package information?

```bash
yum info package-name
```

---

### Q10. How do you check whether a package is installed?

```bash
yum list installed | grep package-name
```

or:

```bash
rpm -q package-name
```

---

### Q11. What is a repository?

A repository is a source containing software packages and package metadata that a package manager can use to install or update software.

---

### Q12. What is a dependency?

A dependency is another software package or library required for a particular application/package to work correctly.

---

### Q13. Why is package management important in DevOps?

It allows engineers to consistently and efficiently install and maintain the software required by servers and applications.

---

### Q14. Difference between `yum install nginx` and `systemctl start nginx`?

```text
yum install nginx
→ installs Nginx

systemctl start nginx
→ starts the Nginx service
```

---

# 🧠 Lesson 8 — MUST KNOW

Before moving forward, make sure you can use:

```text
☑ yum
☑ yum install
☑ yum remove
☑ yum update
☑ yum search
☑ yum info
☑ yum list installed
☑ yum repolist
☑ yum makecache
☑ yum clean all
☑ rpm
☑ rpm -q
☑ rpm -qa
☑ rpm -qf
☑ rpm -ql
☑ Package
☑ Repository
☑ Dependency
☑ RPM
☑ yum vs rpm
☑ yum vs apt
☑ Package management vs systemctl
```

## 🔥 Your 5 commands to memorize first

If you remember nothing else from this lesson, remember:

```bash
sudo yum install nginx
```

```bash
sudo yum remove nginx
```

```bash
sudo yum update
```

```bash
yum search nginx
```

```bash
yum info nginx
```

And remember the DevOps connection:

```text
Linux
  ↓
Package Management
  ↓
Install software
  ↓
Configure software
  ↓
systemctl
  ↓
Run services
  ↓
Deploy application
```
