# 🐧 Linux Fundamentals — Lesson 4
## Essential Linux Commands

Now we move from **Linux theory → actual command-line usage**.

These commands are the foundation for everything you'll do later with **AWS EC2, Docker, Jenkins, Terraform, Ansible, and Kubernetes**.

---

# 1. `pwd` — Print Working Directory ⭐⭐⭐⭐⭐

`pwd` tells you **where you currently are**.

```bash
pwd
```

Example output:

```text
/home/ec2-user
```

Meaning:

> You are currently inside `/home/ec2-user`.

### Remember

**pwd = Present/Print Working Directory**

---

# 2. `ls` — List Files ⭐⭐⭐⭐⭐

Shows files and directories in the current location.

```bash
ls
```

Example:

```text
app.js
index.html
project
```

### Useful options

#### Detailed listing

```bash
ls -l
```

Shows:

- Permissions
- Owner
- Group
- File size
- Date
- Filename

#### Show hidden files

```bash
ls -a
```

#### Detailed + hidden files

```bash
ls -la
```

This is one of the commands you'll use **constantly**.

---

# 3. `cd` — Change Directory ⭐⭐⭐⭐⭐

Used to move between directories.

```bash
cd /etc
```

Check:

```bash
pwd
```

Output:

```text
/etc
```

### Go to parent directory

```bash
cd ..
```

### Go to home directory

```bash
cd ~
```

### Go to previous directory

```bash
cd -
```

Example:

```text
/home/ec2-user
       ↓
/etc
       ↓
cd -
       ↓
/home/ec2-user
```

---

# 4. `mkdir` — Create Directory ⭐⭐⭐⭐⭐

Creates a directory.

```bash
mkdir project
```

Check:

```bash
ls
```

You'll see:

```text
project
```

### Create multiple directories

```bash
mkdir dev test production
```

### Create nested directories

```bash
mkdir -p project/src/app
```

Without `-p`, the parent directories may need to exist first.

---

# 5. `touch` — Create an Empty File ⭐⭐⭐⭐⭐

```bash
touch file.txt
```

Check:

```bash
ls
```

You should see:

```text
file.txt
```

Create multiple files:

```bash
touch app.js index.html style.css
```

### Important

`touch` can also update a file's timestamp.

---

# 6. `cp` — Copy ⭐⭐⭐⭐⭐

Used to copy files/directories.

### Copy a file

```bash
cp file.txt backup.txt
```

Now:

```text
file.txt
backup.txt
```

### Copy file to another directory

```bash
cp file.txt /tmp/
```

### Copy directory

Use `-r`:

```bash
cp -r project project-backup
```

`-r` means **recursive**.

---

# 7. `mv` — Move / Rename ⭐⭐⭐⭐⭐

`mv` is used for both **moving and renaming**.

### Rename

```bash
mv old.txt new.txt
```

### Move

```bash
mv file.txt /tmp/
```

### Move directory

```bash
mv project /opt/
```

### Easy memory trick

```text
cp → Copy
mv → Move / Rename
```

---

# 8. `rm` — Remove ⭐⭐⭐⭐⭐

Deletes files.

```bash
rm file.txt
```

### Remove multiple files

```bash
rm file1.txt file2.txt
```

### Remove directory

```bash
rm -r project
```

### Force removal

```bash
rm -f file.txt
```

### Recursive + force

```bash
rm -rf project
```

⚠️ **Be extremely careful with `rm -rf`.**

Linux generally doesn't provide a recycle-bin safety net for commands like this.

Never blindly run:

```bash
rm -rf /
```

---

# 9. `cat` — Display File Contents ⭐⭐⭐⭐⭐

Displays the contents of a file.

```bash
cat file.txt
```

Example:

```text
Hello Linux
Welcome to DevOps
```

### Create file with content

You can also use:

```bash
echo "Hello Linux" > file.txt
```

Then:

```bash
cat file.txt
```

Output:

```text
Hello Linux
```

---

# 10. `less` — View Large Files ⭐⭐⭐⭐

When a file is very large, `cat` can dump everything onto the screen.

Use:

```bash
less largefile.log
```

You can move through the file.

Useful keys:

```text
Space → Next page
b     → Previous page
↑/↓   → Move
q     → Quit
```

This is especially useful for **log files**.

---

# 11. `head` — First Lines ⭐⭐⭐⭐

Shows the beginning of a file.

```bash
head file.txt
```

By default, it displays the first 10 lines.

Show first 5:

```bash
head -n 5 file.txt
```

---

# 12. `tail` — Last Lines ⭐⭐⭐⭐⭐

Shows the end of a file.

```bash
tail file.txt
```

Show last 20 lines:

```bash
tail -n 20 file.txt
```

### ⭐ Very important for DevOps

```bash
tail -f application.log
```

`-f` means **follow**.

It continuously displays new log entries as they appear.

This is extremely useful when troubleshooting applications.

---

# 13. `echo` — Print Text ⭐⭐⭐⭐⭐

```bash
echo "Hello Linux"
```

Output:

```text
Hello Linux
```

### Create/overwrite a file

```bash
echo "Hello Linux" > file.txt
```

### Append to a file

```bash
echo "Welcome to DevOps" >> file.txt
```

Difference:

```text
>   → overwrite
>>  → append
```

---

# 14. `clear` — Clear Terminal

```bash
clear
```

Clears the visible terminal screen.

Shortcut:

```text
Ctrl + L
```

---

# 15. `history` — Command History ⭐⭐⭐⭐

Shows previously executed commands.

```bash
history
```

Example:

```text
101  pwd
102  ls
103  cd /etc
104  ls -la
```

You can execute a previous command using its number:

```bash
!103
```

---

# 16. `man` — Manual ⭐⭐⭐⭐

Linux provides built-in documentation.

Example:

```bash
man ls
```

You'll get information about the `ls` command.

Exit:

```text
q
```

You can also use:

```bash
ls --help
```

---

# 17. `file` — Identify File Type ⭐⭐⭐

```bash
file myfile
```

Example:

```text
myfile: ASCII text
```

It helps determine what type of data a file contains.

---

# 18. `stat` — Detailed File Information

```bash
stat file.txt
```

It can show:

- File size
- Permissions
- Owner
- Inode
- Access time
- Modification time
- Change time

We'll understand these concepts more deeply later.

---

# 19. Command Combination ⭐⭐⭐⭐⭐

Linux becomes powerful when you combine commands.

Example:

```bash
ls -la /etc
```

Here:

```text
ls      → command
-la     → options
/etc    → target/path
```

Another example:

```bash
cat /etc/hosts
```

---

# 20. Absolute vs Relative Paths

You learned this in Lesson 3.

### Absolute

```bash
cd /var/log
```

Starts from `/`.

### Relative

If you're currently in:

```text
/home/ec2-user
```

You can use:

```bash
cd project
```

---

# 21. Very Important Command Options

You'll frequently see options such as:

```bash
ls -l
ls -a
ls -la
cp -r
rm -r
rm -f
head -n 5
tail -f
```

Don't memorize every option immediately.

Learn the most common ones through practice.

---

# 🧪 Hands-On Lab — Your First Linux Practice

Do this **in order**.

### Step 1 — Check your location

```bash
pwd
```

### Step 2 — List files

```bash
ls
```

### Step 3 — Detailed listing

```bash
ls -l
```

### Step 4 — Hidden files

```bash
ls -la
```

### Step 5 — Create a directory

```bash
mkdir linux-practice
```

### Step 6 — Enter it

```bash
cd linux-practice
```

### Step 7 — Verify

```bash
pwd
```

### Step 8 — Create files

```bash
touch file1.txt file2.txt file3.txt
```

### Step 9 — List them

```bash
ls -l
```

### Step 10 — Add content

```bash
echo "Linux Learning" > file1.txt
```

### Step 11 — Read it

```bash
cat file1.txt
```

### Step 12 — Make a copy

```bash
cp file1.txt backup.txt
```

### Step 13 — Rename it

```bash
mv file2.txt renamed.txt
```

### Step 14 — Check everything

```bash
ls -l
```

### Step 15 — Delete the backup

```bash
rm backup.txt
```

### Step 16 — Go back

```bash
cd ..
```

### Step 17 — Remove the practice directory

```bash
rm -r linux-practice
```

---

# 🎯 Commands You Must Know

For your AWS/DevOps journey, make these automatic:

```text
pwd
ls
cd
mkdir
touch
cp
mv
rm
cat
less
head
tail
echo
clear
history
man
```

### ⭐ Priority

```text
pwd       ⭐⭐⭐⭐⭐
ls        ⭐⭐⭐⭐⭐
cd        ⭐⭐⭐⭐⭐
mkdir     ⭐⭐⭐⭐⭐
touch     ⭐⭐⭐⭐
cp        ⭐⭐⭐⭐⭐
mv        ⭐⭐⭐⭐⭐
rm        ⭐⭐⭐⭐⭐
cat       ⭐⭐⭐⭐⭐
less      ⭐⭐⭐⭐
head      ⭐⭐⭐
tail      ⭐⭐⭐⭐⭐
echo      ⭐⭐⭐⭐⭐
history   ⭐⭐⭐
man       ⭐⭐⭐
```

---

# 🎤 Interview Questions

### Q1. What does `pwd` do?

> `pwd` displays the absolute path of the current working directory.

### Q2. Difference between `cp` and `mv`?

> `cp` copies a file or directory, while `mv` moves or renames it.

### Q3. Difference between `>` and `>>`?

> `>` overwrites the destination, while `>>` appends content to it.

### Q4. What does `rm -rf` do?

> `rm -rf` recursively and forcefully removes files/directories. It should be used carefully because deleted data may not be recoverable through a normal recycle bin.

### Q5. Why is `tail -f` useful in DevOps?

> It continuously displays new lines added to a log file, making it useful for real-time application and server troubleshooting.

### Q6. Difference between `cat` and `less`?

> `cat` displays file contents directly, while `less` allows you to navigate through large files page by page.

### Q7. How do you create a directory?

```bash
mkdir directory_name
```

### Q8. How do you rename a file?

```bash
mv old_name new_name
```

---

# 🧠 Quick Revision

```text
pwd       → Where am I?
ls        → What is here?
cd        → Move
mkdir     → Create directory
touch     → Create file
cp        → Copy
mv        → Move/Rename
rm        → Delete
cat       → Read file
less      → Read large file
head      → Beginning
tail      → End
tail -f   → Follow logs
echo      → Print/write text
clear     → Clear screen
history   → Previous commands
man       → Documentation
```

## ✅ Lesson 4 Complete

- [x] `pwd`
- [x] `ls`
- [x] `cd`
- [x] `mkdir`
- [x] `touch`
- [x] `cp`
- [x] `mv`
- [x] `rm`
- [x] `cat`
- [x] `less`
- [x] `head`
- [x] `tail`
- [x] `echo`
- [x] `clear`
- [x] `history`
- [x] `man`
- [x] Practical lab
- [x] Interview questions
