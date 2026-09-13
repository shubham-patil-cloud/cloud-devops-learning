# 🐧 Linux Fundamentals — Lesson 5
## File & Directory Management

Now we'll go deeper into **how Linux handles files and directories**.

This is important for AWS/DevOps because you'll constantly manage:

- Application files
- Configuration files
- Logs
- Deployment packages
- Scripts
- Backups
- Server directories

---

# 1. Everything in Linux is treated as a file ⭐⭐⭐⭐⭐

A famous Linux concept is:

> **"Everything is a file."**

This means Linux provides a file-like interface for many resources, including:

```text
Regular files
Directories
Devices
Pipes
Sockets
```

For example:

```text
/etc/hosts          → regular file
/home/ec2-user/     → directory
/dev/xvda           → device
```

You don't need to take the phrase literally in every technical situation, but it's an important Linux design concept.

---

# 2. Linux File Types ⭐⭐⭐⭐⭐

Run:

```bash
ls -l
```

You might see:

```text
-rw-r--r--  1 ec2-user ec2-user  120 file.txt
drwxr-xr-x  2 ec2-user ec2-user 4096 project
```

Look at the **first character**.

### `-` → Regular file

```text
-rw-r--r--
```

### `d` → Directory

```text
drwxr-xr-x
```

Other types you'll eventually encounter:

| Symbol | Type |
|---|---|
| `-` | Regular file |
| `d` | Directory |
| `l` | Symbolic link |
| `b` | Block device |
| `c` | Character device |
| `p` | Named pipe |
| `s` | Socket |

For now, focus on:

**`-`, `d`, and `l`.**

---

# 3. Regular Files

Regular files contain normal data.

Examples:

```text
file.txt
app.py
app.js
config.conf
index.html
backup.tar
```

Create one:

```bash
touch app.py
```

Check:

```bash
ls -l app.py
```

---

# 4. Directories

Directories organize files and other directories.

Example:

```text
project/
├── app.py
├── config/
│   └── app.conf
├── logs/
│   └── app.log
└── backup/
```

Create:

```bash
mkdir project
```

---

# 5. Hidden Files ⭐⭐⭐⭐

Linux hides files whose names begin with:

```text
.
```

Example:

```text
.env
.gitignore
.bashrc
.ssh
```

Normal:

```bash
ls
```

may not show them.

Use:

```bash
ls -a
```

to show hidden files.

### Important

```text
.       → current directory
..      → parent directory
```

That's why `ls -a` normally shows:

```text
.
..
```

---

# 6. Creating Nested Directories ⭐⭐⭐⭐⭐

Suppose you want:

```text
project/
└── app/
    └── config/
        └── production/
```

You can use:

```bash
mkdir -p project/app/config/production
```

`-p` creates the required parent directories.

Verify:

```bash
ls -R project
```

`-R` means **recursive**.

---

# 7. Copying Files ⭐⭐⭐⭐⭐

Copy:

```bash
cp file.txt backup.txt
```

Copy to a directory:

```bash
cp file.txt /tmp/
```

Copy multiple files:

```bash
cp file1.txt file2.txt /tmp/
```

---

# 8. Copying Directories ⭐⭐⭐⭐⭐

For directories, use:

```bash
cp -r project project-backup
```

`-r` = recursive.

Example:

```text
project/
├── app.py
└── config/
    └── app.conf
```

After:

```bash
cp -r project project-backup
```

you have:

```text
project/
project-backup/
```

---

# 9. Moving Files ⭐⭐⭐⭐⭐

Move:

```bash
mv app.py /tmp/
```

Move multiple files:

```bash
mv file1.txt file2.txt /tmp/
```

---

# 10. Renaming Files ⭐⭐⭐⭐⭐

Linux doesn't have a separate basic `rename` operation for this purpose.

You normally use:

```bash
mv old.txt new.txt
```

Similarly:

```bash
mv old-directory new-directory
```

renames a directory.

---

# 11. Deleting Files ⭐⭐⭐⭐⭐

Delete:

```bash
rm file.txt
```

Delete multiple:

```bash
rm file1.txt file2.txt
```

Delete directory:

```bash
rm -r project
```

Force:

```bash
rm -f file.txt
```

Recursive + force:

```bash
rm -rf project
```

⚠️ **Be very careful with `rm -rf` on servers.**

Always verify your path before running destructive commands.

---

# 12. Wildcards ⭐⭐⭐⭐⭐

Wildcards allow you to work with multiple files.

## `*`

Matches zero or more characters.

Suppose:

```text
app.txt
app.log
app.py
test.txt
```

Run:

```bash
ls app*
```

You'll get:

```text
app.txt
app.log
app.py
```

---

## `?`

Matches exactly one character.

Example:

```text
file1.txt
file2.txt
file3.txt
```

```bash
ls file?.txt
```

matches those files.

---

## `[ ]`

Matches characters from a specified set/range.

Example:

```bash
ls file[123].txt
```

matches:

```text
file1.txt
file2.txt
file3.txt
```

---

# 13. `find` ⭐⭐⭐⭐⭐

This is **very important for DevOps**.

`find` searches for files and directories.

Basic:

```bash
find /home -name "app.py"
```

Search from current directory:

```bash
find . -name "app.py"
```

---

## Find by name

```bash
find /var/log -name "*.log"
```

Finds `.log` files.

---

## Find directories

```bash
find /home -type d -name "project"
```

`-type d` means directory.

---

## Find regular files

```bash
find /home -type f -name "*.txt"
```

`-type f` means regular file.

---

# 14. Find Files by Size ⭐⭐⭐⭐

Example:

```bash
find /var/log -type f -size +100M
```

Meaning:

> Find regular files larger than 100 MB.

This can be useful when troubleshooting a **disk-full problem**.

---

# 15. Find Files by Modification Time ⭐⭐⭐⭐

Find files modified within the last day:

```bash
find /var/log -type f -mtime -1
```

Useful for troubleshooting:

> "Which files changed recently?"

---

# 16. `locate`

Another command for finding files:

```bash
locate nginx.conf
```

It's often faster than `find` because it searches a prebuilt database.

However, the database may not immediately contain newly created files.

For that reason, `find` is generally more important to learn first.

---

# 17. `which` ⭐⭐⭐⭐

`which` tells you where an executable command is located in your `PATH`.

Example:

```bash
which python
```

Possible output:

```text
/usr/bin/python
```

Another:

```bash
which bash
```

Possible:

```text
/usr/bin/bash
```

---

# 18. `whereis`

`whereis` can locate binaries, source files, and manual pages associated with a command.

Example:

```bash
whereis bash
```

Possible output:

```text
bash: /usr/bin/bash /usr/share/man/man1/bash.1.gz
```

### Difference

```text
which    → executable in PATH
whereis  → binary + source + man page locations
find     → filesystem search
locate   → database-based file search
```

---

# 19. `file`

Use:

```bash
file filename
```

Example:

```bash
file app.py
```

Possible output:

```text
app.py: Python script, ASCII text executable
```

Useful when you don't know what type of file something actually is.

---

# 20. `stat` ⭐⭐⭐⭐

`stat` provides detailed information about a file.

```bash
stat file.txt
```

You'll see information such as:

- File size
- Permissions
- Owner
- Inode
- Access time
- Modification time
- Change time

Example concepts:

```text
Access      → last accessed
Modify      → content modified
Change      → metadata changed
```

We'll study these more deeply later.

---

# 21. Symbolic Links ⭐⭐⭐⭐⭐

A symbolic link, or **symlink**, is a reference/pointer to another file or directory.

Create one:

```bash
ln -s original.txt link.txt
```

Check:

```bash
ls -l
```

You'll see something similar to:

```text
link.txt -> original.txt
```

Think:

```text
link.txt
    ↓
original.txt
```

If applications access `link.txt`, they're redirected to the target.

---

# 22. Why Symlinks Matter in DevOps

Symlinks are commonly used for:

- Application versions
- Configuration
- Log locations
- Software installations

Example:

```text
/opt/myapp/
├── releases/
│   ├── v1
│   └── v2
└── current -> releases/v2
```

When deploying v3:

```text
current -> releases/v3
```

You can change the active version without moving all the application files.

---

# 23. Hard Links

Linux also supports hard links.

```bash
ln original.txt hardlink.txt
```

A hard link refers to the same underlying inode/data as the original file.

For now, remember:

```text
Symbolic link → points to a path
Hard link     → another directory entry for the same inode
```

We'll study **inodes, hard links and symlinks** more deeply later.

---

# 24. Useful Recursive Commands

### List recursively

```bash
ls -R
```

### Copy recursively

```bash
cp -r
```

### Remove recursively

```bash
rm -r
```

### Search recursively

```bash
find
```

The concept of **recursion** means operating through nested directories/files.

---

# 🧪 Hands-On Lab

Let's build a small application structure.

## Step 1 — Create project

```bash
mkdir linux-project
```

## Step 2 — Enter it

```bash
cd linux-project
```

## Step 3 — Create directories

```bash
mkdir app logs backup config
```

## Step 4 — Create files

```bash
touch app/app.py
touch app/index.html
touch logs/app.log
touch config/app.conf
```

Your structure:

```text
linux-project/
├── app/
│   ├── app.py
│   └── index.html
├── logs/
│   └── app.log
├── backup/
└── config/
    └── app.conf
```

## Step 5 — Check

```bash
ls -R
```

## Step 6 — Add configuration

```bash
echo "PORT=8080" > config/app.conf
```

Check:

```bash
cat config/app.conf
```

## Step 7 — Create backup

```bash
cp -r app backup/
```

Now:

```text
backup/
└── app/
    ├── app.py
    └── index.html
```

## Step 8 — Rename a file

```bash
mv app/index.html app/home.html
```

## Step 9 — Find Python files

```bash
find . -type f -name "*.py"
```

## Step 10 — Create symlink

```bash
ln -s config/app.conf app.conf
```

Check:

```bash
ls -l
```

You'll see:

```text
app.conf -> config/app.conf
```

---

# 🎤 Interview Questions

### Q1. What is the difference between a file and a directory?

> A file stores data, while a directory organizes files and other directories.

### Q2. How do you list hidden files?

```bash
ls -la
```

### Q3. How do you copy a directory?

```bash
cp -r source destination
```

### Q4. How do you find a file?

```bash
find /path -name "filename"
```

### Q5. Difference between `find` and `locate`?

> `find` searches the filesystem directly, while `locate` searches a prebuilt file database and is generally faster but may not immediately reflect newly created files.

### Q6. Difference between `which` and `whereis`?

> `which` identifies the executable found through the user's `PATH`, while `whereis` can locate the binary, source, and manual pages associated with a command.

### Q7. What is a symbolic link?

> A symbolic link is a special file that points to another file or directory.

Example:

```bash
ln -s original.txt link.txt
```

### Q8. What does `-r` mean?

> `-r` generally means recursive, allowing a command to operate through directories and their contents.

### Q9. What does `*` mean in Linux?

> `*` is a wildcard that matches zero or more characters in filenames.

---

# 🧠 Quick Revision

```text
ls -l       → Detailed files
ls -a       → Hidden files
ls -la      → Detailed + hidden

mkdir       → Create directory
mkdir -p    → Create nested directories

cp          → Copy
cp -r       → Copy directory

mv          → Move / Rename

rm          → Delete
rm -r       → Delete directory recursively
rm -f       → Force
rm -rf      → Recursive + force

find        → Search filesystem
locate      → Search file database
which       → Find executable
whereis     → Find binary/source/man

file        → Identify file type
stat        → Detailed file metadata

ln -s       → Create symbolic link
```

---

# ⭐ What You Should Be Able to Do Now

After Lessons 1–5, you should understand:

```text
Linux
 ↓
Architecture
 ↓
Filesystem
 ↓
Directories
 ↓
Files
 ↓
Commands
 ↓
File management
```

And you should be comfortable with:

```bash
pwd
ls
cd
mkdir
touch
cp
mv
rm
cat
find
which
whereis
file
stat
ln
```

## ✅ Lesson 5 Complete

- [x] Linux file types
- [x] Regular files
- [x] Directories
- [x] Hidden files
- [x] Nested directories
- [x] Copying
- [x] Moving
- [x] Renaming
- [x] Deleting
- [x] Wildcards
- [x] `find`
- [x] `locate`
- [x] `which`
- [x] `whereis`
- [x] `file`
- [x] `stat`
- [x] Symbolic links
- [x] Hard links basics
- [x] Practical lab
- [x] Interview questions

