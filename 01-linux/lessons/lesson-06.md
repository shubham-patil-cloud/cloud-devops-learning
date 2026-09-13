# 🐧 Lesson 6 — Linux File Permissions & Ownership

File permissions are **extremely important for Cloud/DevOps** because you will constantly work with:

- SSH keys
- EC2 servers
- Web servers
- Application files
- Logs
- Scripts
- Configuration files
- Users and groups
- `sudo`
- Docker/Kubernetes volumes
- Deployment pipelines

---

# 1. What are Linux Permissions?

Linux controls **who can access a file or directory and what they can do with it**.

There are 3 basic permissions:

| Permission | Symbol | Meaning |
|---|---|---|
| Read | `r` | View/read content |
| Write | `w` | Modify content |
| Execute | `x` | Execute/run file |

And there are 3 categories of users:

| Category | Symbol | Meaning |
|---|---|---|
| User | `u` | File owner |
| Group | `g` | Users belonging to file's group |
| Others | `o` | Everyone else |

---

# 2. Understanding `ls -l`

Run:

```bash
ls -l
```

Example:

```text
-rwxr-xr-- 1 shubham developers 1200 Sep 12 script.sh
```

Break it down:

```text
-rwxr-xr--
│└──┬──┘└─┬─┘
│   │     │
│   │     └── Others
│   └──────── Group
└──────────── User
```

More clearly:

```text
- rwx r-x r--
  │   │   │
  │   │   └── Others
  │   └────── Group
  └────────── Owner
```

So:

```text
- rwx r-x r--
  │ │   │   │
  │ │   │   └── Others: read
  │ │   └────── Group: read + execute
  │ └────────── Owner: read + write + execute
  └──────────── File type
```

---

# 3. File Type

The first character tells you the file type.

```text
-rwxr-xr--
```

First character:

```text
-
```

means regular file.

Common types:

| Symbol | Type |
|---|---|
| `-` | Regular file |
| `d` | Directory |
| `l` | Symbolic link |
| `b` | Block device |
| `c` | Character device |

Example:

```bash
ls -l
```

Could show:

```text
-rw-r--r-- file.txt
drwxr-xr-x myfolder
lrwxrwxrwx link -> file.txt
```

---

# 4. Read Permission `r`

For a **file**:

```text
r = read the file
```

Example:

```bash
cat file.txt
```

requires read permission.

For a **directory**:

```text
r = list directory contents
```

Example:

```bash
ls myfolder
```

requires read permission on the directory.

---

# 5. Write Permission `w`

For a **file**:

```text
w = modify file content
```

Example:

```bash
echo "Hello" >> file.txt
```

For a **directory**:

```text
w = create/delete/rename entries
```

This is an important interview point.

### Directory `w` does NOT mean "edit every file inside it."

It means you can modify the **directory's entries**.

---

# 6. Execute Permission `x`

For a **file**:

```text
x = execute/run the file
```

Example:

```bash
./script.sh
```

For a **directory**:

```text
x = enter/traverse the directory
```

Example:

```bash
cd myfolder
```

Without execute permission on a directory, you generally cannot traverse it.

---

# 7. The Three Permission Groups

Suppose:

```text
-rwxr-xr--
```

Separate it:

```text
rwx | r-x | r--
```

### User / Owner

```text
rwx
```

Owner can:

```text
read
write
execute
```

### Group

```text
r-x
```

Group can:

```text
read
execute
```

but cannot write.

### Others

```text
r--
```

Others can:

```text
read
```

but cannot write or execute.

---

# 8. Linux Ownership

Every file normally has:

1. **Owner/User**
2. **Group**

Check with:

```bash
ls -l
```

Example:

```text
-rw-r--r-- 1 ec2-user developers 500 app.py
```

Here:

```text
Owner = ec2-user
Group = developers
```

---

# 9. `chown` — Change Owner

Syntax:

```bash
chown USER file
```

Example:

```bash
sudo chown shubham app.py
```

Change owner and group:

```bash
sudo chown shubham:developers app.py
```

Check:

```bash
ls -l app.py
```

---

# 10. `chgrp` — Change Group

Syntax:

```bash
chgrp GROUP file
```

Example:

```bash
sudo chgrp developers app.py
```

Verify:

```bash
ls -l app.py
```

---

# 11. Recursive Ownership

Suppose you have:

```text
/var/www/html/
```

with many files.

You can change ownership recursively:

```bash
sudo chown -R ec2-user:developers /var/www/html
```

`-R` means:

```text
Recursive
```

It applies to the directory and everything inside it.

⚠️ Be careful with recursive `chown` on system directories.

---

# 12. `chmod` — Change Permissions

`chmod` means:

> **Change Mode**

Basic syntax:

```bash
chmod permissions file
```

Example:

```bash
chmod 755 script.sh
```

---

# 13. Numeric Permissions

This is extremely important for interviews and real work.

Linux permissions have numeric values:

| Permission | Value |
|---|---:|
| `r` | 4 |
| `w` | 2 |
| `x` | 1 |
| No permission | 0 |

Add them together.

### `rwx`

```text
r = 4
w = 2
x = 1

4 + 2 + 1 = 7
```

Therefore:

```text
rwx = 7
```

### `rw-`

```text
4 + 2 = 6
```

Therefore:

```text
rw- = 6
```

### `r-x`

```text
4 + 1 = 5
```

Therefore:

```text
r-x = 5
```

### `r--`

```text
4 = 4
```

---

# 14. Most Important Permission Numbers

| Number | Permission |
|---:|---|
| `7` | `rwx` |
| `6` | `rw-` |
| `5` | `r-x` |
| `4` | `r--` |
| `3` | `-wx` |
| `2` | `-w-` |
| `1` | `--x` |
| `0` | `---` |

---

# 15. Understanding `755`

```bash
chmod 755 script.sh
```

Break it:

```text
7   5   5
│   │   │
│   │   └── Others
│   └────── Group
└────────── Owner
```

Convert:

```text
7 = rwx
5 = r-x
5 = r-x
```

Therefore:

```text
rwxr-xr-x
```

Meaning:

> Owner can read/write/execute; group and others can read/execute.

Very commonly used for:

```text
Shell scripts
Executable applications
Directories
```

---

# 16. Understanding `644`

```bash
chmod 644 app.py
```

Convert:

```text
6 = rw-
4 = r--
4 = r--
```

Therefore:

```text
rw-r--r--
```

Meaning:

```text
Owner  → read + write
Group  → read
Others → read
```

Common for:

```text
HTML
CSS
JavaScript
configuration/text files
application source files
```

---

# 17. Understanding `700`

```bash
chmod 700 private.sh
```

Convert:

```text
7 = rwx
0 = ---
0 = ---
```

Result:

```text
rwx------
```

Only the owner has access.

Useful for:

```text
private scripts
private directories
sensitive files
```

---

# 18. SSH Key Example 🔐

One of the most important Cloud examples.

Suppose:

```text
my-key.pem
```

AWS SSH commonly requires the private key to be accessible only by the owner.

Set:

```bash
chmod 400 my-key.pem
```

or commonly:

```bash
chmod 600 my-key.pem
```

Then:

```bash
ssh -i my-key.pem ec2-user@SERVER_IP
```

If permissions are too open, SSH may reject the key with an error similar to:

```text
WARNING: UNPROTECTED PRIVATE KEY FILE!
```

This is a **real-world Cloud/DevOps permission problem**.

---

# 19. Symbolic `chmod`

You don't always need numbers.

You can use:

```text
u = user
g = group
o = others
a = all
```

### Add execute permission to owner

```bash
chmod u+x script.sh
```

### Add execute permission to everyone

```bash
chmod a+x script.sh
```

### Remove write permission from others

```bash
chmod o-w file.txt
```

### Add read permission to group

```bash
chmod g+r file.txt
```

### Give owner read/write

```bash
chmod u+rw file.txt
```

---

# 20. Multiple Permission Changes

Example:

```bash
chmod u+x,g+r,o-r script.sh
```

Meaning:

```text
Owner  → add execute
Group  → add read
Others → remove read
```

---

# 21. `sudo`

`sudo` means:

> Execute a command with elevated privileges.

Example:

```bash
sudo yum update
```

or:

```bash
sudo chown root:root config.conf
```

Normal user:

```bash
ls /root
```

may return:

```text
Permission denied
```

With appropriate privileges:

```bash
sudo ls /root
```

---

# 22. Root User

Linux has a special administrative user:

```text
root
```

Root has extremely high privileges.

Check current user:

```bash
whoami
```

Example:

```text
ec2-user
```

Switch to root shell:

```bash
sudo -i
```

Check:

```bash
whoami
```

Output:

```text
root
```

Exit:

```bash
exit
```

### Important

Don't use root unnecessarily.

For DevOps:

> Use the least privilege necessary.

This is also an important security principle.

---

# 23. Practical Lab 🔥

Let's create a complete permissions exercise.

### Step 1 — Create directory

```bash
mkdir permissions-lab
cd permissions-lab
```

### Step 2 — Create files

```bash
touch public.txt private.txt script.sh
```

Check:

```bash
ls -l
```

---

### Step 3 — Set normal file permissions

```bash
chmod 644 public.txt
```

Check:

```bash
ls -l public.txt
```

Expected:

```text
-rw-r--r--
```

---

### Step 4 — Set private file

```bash
chmod 600 private.txt
```

Expected:

```text
-rw-------
```

---

### Step 5 — Make script executable

Add some content:

```bash
echo '#!/bin/bash' > script.sh
echo 'echo "Linux Permissions Lab"' >> script.sh
```

Set permission:

```bash
chmod 755 script.sh
```

Check:

```bash
ls -l script.sh
```

Then run:

```bash
./script.sh
```

Expected:

```text
Linux Permissions Lab
```

---

# 24. Test Permission Failure

Remove execute permission:

```bash
chmod 644 script.sh
```

Try:

```bash
./script.sh
```

You may get:

```text
Permission denied
```

Now fix it:

```bash
chmod +x script.sh
```

Run:

```bash
./script.sh
```

---

# 25. Directory Permissions

Create:

```bash
mkdir testdir
```

Set:

```bash
chmod 755 testdir
```

Check:

```bash
ls -ld testdir
```

Notice the first character:

```text
d
```

Example:

```text
drwxr-xr-x
```

Because it is a directory.

---

# 26. Important Difference: File vs Directory

This is a **very common interview question**.

### File

```text
r → read content
w → modify content
x → execute file
```

### Directory

```text
r → list contents
w → create/delete/rename entries
x → enter/traverse directory
```

Remember this table:

| Permission | File | Directory |
|---|---|---|
| `r` | Read content | List contents |
| `w` | Modify content | Create/delete/rename |
| `x` | Execute | Enter/traverse |

---

# 27. Real AWS Web Server Example

Imagine:

```text
/var/www/html/
```

contains:

```text
index.html
style.css
app.js
```

You might check:

```bash
ls -l /var/www/html
```

Suppose:

```text
-rw-r--r-- index.html
-rw-r--r-- style.css
-rw-r--r-- app.js
```

This means:

```text
Owner → read/write
Group → read
Others → read
```

A web server can generally **read** these files without needing write permission.

That's good security practice.

---

# 28. Real DevOps Example — Deployment Directory

Suppose:

```text
/opt/myapp/
```

belongs to:

```text
deploy:developers
```

You could use:

```bash
sudo chown -R deploy:developers /opt/myapp
```

Then:

```bash
sudo chmod -R 755 /opt/myapp
```

However, don't blindly use `chmod -R 755` on an entire application tree. Some files should not be executable, and sensitive files may need tighter permissions.

A better approach is to assign permissions according to the actual role of each file/directory.

---

# 29. Permission Troubleshooting Flow

When you get:

```text
Permission denied
```

Don't immediately use:

```bash
chmod 777
```

Instead check:

### Step 1 — Current user

```bash
whoami
```

### Step 2 — File permissions

```bash
ls -l file
```

### Step 3 — Ownership

```bash
ls -l file
```

Example:

```text
-rw------- 1 root root config.txt
```

### Step 4 — Directory permissions

Check the path:

```bash
ls -ld /path
```

### Step 5 — Change owner if appropriate

```bash
sudo chown user:group file
```

### Step 6 — Change permissions if appropriate

```bash
chmod 640 file
```

### Step 7 — Use `sudo` only when necessary

```bash
sudo command
```

---

# 30. 🚨 Never Use `chmod 777` Blindly

You will see:

```bash
chmod 777 file
```

This gives:

```text
rwx rwx rwx
```

Meaning:

```text
Owner  → everything
Group  → everything
Others → everything
```

It is usually **far too permissive**.

Bad approach:

```bash
chmod 777 /var/www/html
```

Better:

```text
Give only the permissions actually required.
```

This is the **principle of least privilege**.

---

# 31. Important Permissions Cheat Sheet

Memorize these:

```text
644 → rw-r--r--
755 → rwxr-xr-x
700 → rwx------
600 → rw-------
400 → r--------
```

### Typical usage

| Permission | Common use |
|---|---|
| `644` | Normal files |
| `755` | Executable files/directories |
| `700` | Private directories/scripts |
| `600` | Sensitive/private files |
| `400` | Read-only private files / some SSH key situations |

---

# 32. Commands You MUST Know

```bash
ls -l
```

```bash
chmod
```

```bash
chown
```

```bash
chgrp
```

```bash
sudo
```

```bash
whoami
```

```bash
id
```

```bash
groups
```

Useful checks:

```bash
id
```

shows user and group information.

```bash
groups
```

shows groups the current user belongs to.

---

# 33. Interview Questions 🎯

### Q1. What are Linux file permissions?

Permissions control who can read, write, or execute a file or access a directory.

---

### Q2. What are the three basic Linux permissions?

```text
r = read
w = write
x = execute
```

---

### Q3. What are user, group and others?

```text
user   → owner
group  → members of file's group
others → everyone else
```

---

### Q4. What does `755` mean?

```text
rwxr-xr-x
```

Owner:

```text
rwx
```

Group:

```text
r-x
```

Others:

```text
r-x
```

---

### Q5. What does `644` mean?

```text
rw-r--r--
```

Owner can read/write; group and others can read.

---

### Q6. Difference between `chmod` and `chown`?

```text
chmod → changes permissions
chown → changes ownership
```

---

### Q7. Difference between `chown` and `chgrp`?

```text
chown  → changes owner, and optionally group
chgrp  → changes group
```

---

### Q8. What does `sudo` do?

It allows an authorized user to execute a command with elevated privileges.

---

### Q9. What does `chmod 777` mean?

```text
rwxrwxrwx
```

Everyone has read, write and execute permission.

It is generally insecure unless there is a specific reason.

---

### Q10. What does `chmod +x script.sh` do?

Adds execute permission to the file according to the existing `chmod` default behavior.

---

### Q11. What is the difference between `rwx` on a file and directory?

For a file:

```text
r → read
w → modify
x → execute
```

For a directory:

```text
r → list
w → create/delete/rename
x → enter/traverse
```

---

### Q12. Why are permissions important in AWS?

They protect:

- SSH private keys
- Application files
- Configuration files
- Logs
- Credentials
- Web-server files
- Deployment directories

---

# 🧠 Lesson 6 — Must Remember

Before moving to Lesson 7, you should be comfortable with:

```text
☑ r / w / x
☑ user / group / others
☑ ls -l
☑ File ownership
☑ chmod
☑ chmod 755
☑ chmod 644
☑ chmod 700
☑ chmod 600
☑ chmod 400
☑ chmod u+x
☑ chown
☑ chgrp
☑ sudo
☑ root
☑ whoami
☑ id
☑ groups
☑ File vs directory permissions
☑ Permission denied troubleshooting
☑ Why chmod 777 is dangerous
☑ SSH key permissions
```

## 🔥 Mini Challenge

Without looking at the answers, explain these:

```bash
chmod 755 script.sh
chmod 644 index.html
chmod 600 secret.txt
chmod 700 private/
sudo chown ec2-user:developers app.py
```

If you can explain **exactly what each command does and why**, you've understood the core of Lesson 6.
