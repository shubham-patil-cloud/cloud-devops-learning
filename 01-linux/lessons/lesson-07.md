# 🐧 Lesson 7 — Linux Users & Groups

Users and groups are a **core Linux + Cloud/DevOps topic**.

They directly connect with the previous lesson:

> **Users + Groups + Ownership + Permissions = Linux Security**

You will use them when managing **AWS EC2 servers, application deployments, web servers, SSH access, Docker, Jenkins, Ansible and Kubernetes**.

---

# 1. What is a Linux User?

A **user** is an account that can log in to and interact with a Linux system.

Examples:

```text
root
ec2-user
ubuntu
jenkins
deploy
developer
```

Each user has:

- Username
- User ID (UID)
- Primary group
- Home directory
- Login shell
- Permissions

Check your current user:

```bash
whoami
```

Example:

```text
ec2-user
```

---

# 2. Root User

`root` is the Linux superuser.

Root has very high privileges and can:

- Create users
- Delete users
- Change permissions
- Install software
- Modify system files
- Start/stop services
- Access protected directories

Check:

```bash
whoami
```

If output is:

```text
root
```

you are currently root.

---

# 3. Normal User vs Root

| Root | Normal User |
|---|---|
| UID usually `0` | UID usually `1000+` |
| Full administrative privileges | Limited privileges |
| Can access most system resources | Access controlled by permissions |
| Can create/delete users | Usually cannot without `sudo` |
| Can modify system configuration | Limited |

In DevOps:

> Avoid working as root unnecessarily.

Use a normal user + `sudo` when administrative privileges are required.

---

# 4. What is a UID?

UID = **User ID**

Linux internally identifies users using numbers.

Check:

```bash
id
```

Example:

```text
uid=1000(ec2-user) gid=1000(ec2-user) groups=1000(ec2-user)
```

Here:

```text
UID = 1000
```

Linux uses the UID internally rather than relying only on the username.

---

# 5. What is a Group?

A **group** is a collection of users.

Instead of giving permissions individually to every user, you can give permissions to a group.

Example:

```text
developers
   │
   ├── rahul
   ├── amit
   └── shubham
```

If a directory belongs to the `developers` group, all members can receive the permissions assigned to that group.

This is extremely useful in organizations.

---

# 6. Why Groups Matter in DevOps

Imagine your server has:

```text
developer1
developer2
developer3
```

All three need access to:

```text
/opt/myapp
```

Instead of doing:

```text
developer1 → permissions
developer2 → permissions
developer3 → permissions
```

Create:

```text
developers
```

Add everyone:

```text
developer1
developer2
developer3
```

Then assign the directory to:

```text
developers
```

Much easier to manage.

---

# 7. Check User Information

Use:

```bash
id username
```

Example:

```bash
id ec2-user
```

Output may look like:

```text
uid=1000(ec2-user) gid=1000(ec2-user) groups=1000(ec2-user),10(wheel)
```

This tells you:

```text
UID
Primary GID
Supplementary groups
```

---

# 8. `groups`

Check your groups:

```bash
groups
```

Example:

```text
ec2-user wheel
```

For another user:

```bash
groups ec2-user
```

---

# 9. Primary Group vs Supplementary Groups

This is important.

A user normally has:

### Primary group

The main group associated with the user.

### Supplementary groups

Additional groups the user belongs to.

Example:

```text
User: shubham

Primary group:
developers

Additional groups:
docker
wheel
```

This means Shubham can receive permissions through all those groups.

---

# 10. `/etc/passwd`

Linux stores basic user-account information in:

```bash
/etc/passwd
```

View it:

```bash
cat /etc/passwd
```

Example:

```text
ec2-user:x:1000:1000:EC2 User:/home/ec2-user:/bin/bash
```

The format is:

```text
username:password:UID:GID:GECOS:home:shell
```

For example:

```text
ec2-user
   ↓
x
   ↓
1000
   ↓
1000
   ↓
EC2 User
   ↓
/home/ec2-user
   ↓
/bin/bash
```

---

# 11. What Does `x` Mean in `/etc/passwd`?

You might see:

```text
ec2-user:x:1000:1000:...
```

The `x` does **not** mean the actual password is stored there.

Modern Linux systems generally store password hashes in:

```text
/etc/shadow
```

---

# 12. `/etc/shadow`

View:

```bash
sudo cat /etc/shadow
```

This file contains sensitive authentication information such as password hashes and account-related password data.

It is normally restricted.

You may see:

```text
root:...
ec2-user:...
```

Do **not** modify `/etc/shadow` manually unless you know exactly what you're doing.

---

# 13. `/etc/group`

Groups are defined in:

```text
/etc/group
```

View:

```bash
cat /etc/group
```

Example:

```text
developers:x:1001:shubham,rahul,amit
```

Format:

```text
group_name:password:GID:members
```

So:

```text
Group = developers
GID = 1001
Members = shubham, rahul, amit
```

---

# 14. Create a User

Command:

```bash
sudo useradd username
```

Example:

```bash
sudo useradd developer
```

Check:

```bash
id developer
```

---

# 15. Create User with Home Directory

A common command:

```bash
sudo useradd -m developer
```

`-m` means:

> Create the user's home directory.

Usually:

```text
/home/developer
```

Check:

```bash
ls -ld /home/developer
```

---

# 16. Create User with Specific Shell

Example:

```bash
sudo useradd -m -s /bin/bash developer
```

Meaning:

```text
-m → create home directory
-s → specify login shell
```

---

# 17. Set User Password

Use:

```bash
sudo passwd developer
```

You'll be prompted to enter the password.

Example:

```text
New password:
Retype new password:
```

---

# 18. Verify User

```bash
id developer
```

And:

```bash
grep developer /etc/passwd
```

You may see:

```text
developer:x:1001:1001::/home/developer:/bin/bash
```

---

# 19. Switch User

Use:

```bash
su - developer
```

Then:

```bash
whoami
```

Output:

```text
developer
```

The `-` loads the user's login environment.

Return to your previous user:

```bash
exit
```

---

# 20. `sudo -u`

You can execute a command as another user.

Example:

```bash
sudo -u developer whoami
```

Output:

```text
developer
```

This is useful when testing whether a particular user can access something.

---

# 21. Create a Group

Command:

```bash
sudo groupadd developers
```

Check:

```bash
grep developers /etc/group
```

Example:

```text
developers:x:1001:
```

---

# 22. Add User to Group

Use:

```bash
sudo usermod -aG developers developer
```

Break it down:

```text
usermod → modify user
-a      → append
-G      → supplementary group
```

### Very important:

Use:

```bash
-aG
```

not just:

```bash
-G
```

because `-G` without `-a` can replace the user's existing supplementary groups.

---

# 23. Verify Group Membership

```bash
groups developer
```

Example:

```text
developer : developer developers
```

Or:

```bash
id developer
```

You should see:

```text
groups=...,developers
```

---

# 24. Group Membership May Need a New Login

After adding a user to a group:

```bash
sudo usermod -aG developers developer
```

The currently active session may not immediately reflect the new group.

You can:

- Log out and log in again
- Start a new login session

Or test with:

```bash
su - developer
```

---

# 25. Remove User from a Group

On systems using GNU `usermod`, you can use:

```bash
sudo gpasswd -d developer developers
```

Then:

```bash
groups developer
```

---

# 26. Delete a User

Basic:

```bash
sudo userdel developer
```

This removes the account but may leave the home directory.

To remove the user's home directory as well:

```bash
sudo userdel -r developer
```

⚠️ Be careful with `-r`.

It can remove the user's home directory and mail-related files.

---

# 27. Modify a User

`usermod` is used to modify an existing user.

Example:

```bash
sudo usermod -s /bin/bash developer
```

Change home directory:

```bash
sudo usermod -d /home/newdeveloper developer
```

Add supplementary group:

```bash
sudo usermod -aG developers developer
```

---

# 28. Rename a User

Example:

```bash
sudo usermod -l newname oldname
```

Be careful: changing usernames can require additional changes to home directories, ownership, services, SSH configuration, etc.

For real production systems, plan this carefully.

---

# 29. `who`

Shows users currently logged in:

```bash
who
```

Example:

```text
ec2-user pts/0 2026-09-12 17:30
```

---

# 30. `w`

Another useful command:

```bash
w
```

It shows:

- Logged-in users
- Login time
- Terminal
- Idle time
- Current activity

Useful when troubleshooting shared servers.

---

# 31. `last`

Shows login history:

```bash
last
```

Useful for checking previous login activity.

---

# 32. `whoami` vs `who`

Very important:

### `whoami`

```bash
whoami
```

Answers:

> **Who am I currently logged in as?**

### `who`

```bash
who
```

Answers:

> **Who is currently logged into this system?**

---

# 33. AWS EC2 Example 🚀

Suppose you launch an EC2 instance.

Amazon Linux commonly uses:

```text
ec2-user
```

Ubuntu commonly uses:

```text
ubuntu
```

You SSH into Amazon Linux:

```bash
ssh -i my-key.pem ec2-user@SERVER_IP
```

Then:

```bash
whoami
```

Output:

```text
ec2-user
```

Check:

```bash
id
```

You can use `sudo` when administrative access is required:

```bash
sudo yum update
```

---

# 34. Real DevOps Example — Developer Access

Suppose you have:

```text
developer1
developer2
developer3
```

Create:

```bash
sudo groupadd developers
```

Add users:

```bash
sudo usermod -aG developers developer1
sudo usermod -aG developers developer2
sudo usermod -aG developers developer3
```

Create application directory:

```bash
sudo mkdir /opt/myapp
```

Set group ownership:

```bash
sudo chown -R root:developers /opt/myapp
```

Now the directory can be managed through the `developers` group, subject to its permission bits.

---

# 35. Combining Users + Groups + Permissions

This connects directly to **Lesson 6**.

Suppose:

```text
/opt/myapp
```

has:

```text
Owner = root
Group = developers
```

Permissions:

```text
drwxrwxr-x
```

Breakdown:

```text
Owner     → rwx
Group     → rwx
Others    → r-x
```

Therefore:

```text
root              → read/write/execute
developers group  → read/write/execute
others            → read/execute
```

This is much better than making everything:

```text
777
```

---

# 36. `wheel` Group

On many Red Hat-family distributions, including Amazon Linux, the `wheel` group is commonly associated with users who are allowed to use `sudo`, depending on the system's `sudoers` configuration.

Check:

```bash
groups
```

You may see:

```text
ec2-user wheel
```

You can inspect sudo configuration with:

```bash
sudo visudo
```

⚠️ Don't casually edit `/etc/sudoers` with a normal text editor. `visudo` checks syntax before saving.

---

# 37. `sudoers`

Linux controls sudo access through sudoers configuration.

The main file is:

```text
/etc/sudoers
```

Additional configuration can exist under:

```text
/etc/sudoers.d/
```

For example, organizations may give a deployment user specific administrative commands rather than unrestricted root access.

This is an important security concept:

> **Least privilege**

---

# 38. User Management Workflow

A typical DevOps workflow:

```text
Create user
    ↓
Create group
    ↓
Add user to group
    ↓
Set ownership
    ↓
Set permissions
    ↓
Configure sudo if required
    ↓
Test access
```

Example:

```bash
sudo useradd -m developer
sudo groupadd developers
sudo usermod -aG developers developer
```

Then:

```bash
id developer
```

---

# 39. Practical Lab 🔥

Let's create a realistic DevOps user-management lab.

### Step 1 — Create group

```bash
sudo groupadd devops
```

---

### Step 2 — Create users

```bash
sudo useradd -m dev1
sudo useradd -m dev2
```

---

### Step 3 — Set passwords

```bash
sudo passwd dev1
sudo passwd dev2
```

---

### Step 4 — Add users to group

```bash
sudo usermod -aG devops dev1
sudo usermod -aG devops dev2
```

---

### Step 5 — Verify

```bash
id dev1
```

```bash
id dev2
```

You should see `devops` among their groups.

---

### Step 6 — Create application directory

```bash
sudo mkdir /opt/devops-app
```

---

### Step 7 — Change group ownership

```bash
sudo chown root:devops /opt/devops-app
```

---

### Step 8 — Set directory permissions

```bash
sudo chmod 775 /opt/devops-app
```

Check:

```bash
ls -ld /opt/devops-app
```

Expected structure:

```text
drwxrwxr-x
```

Now:

```text
root      → rwx
devops    → rwx
others    → r-x
```

---

### Step 9 — Test as `dev1`

```bash
sudo -u dev1 mkdir /opt/devops-app/test
```

If the permissions and group membership are correctly active, `dev1` should be able to create the directory.

---

### Step 10 — Test as `dev2`

```bash
sudo -u dev2 touch /opt/devops-app/app.txt
```

Now:

```bash
ls -l /opt/devops-app
```

This demonstrates the actual relationship:

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

# 40. Important Commands Cheat Sheet

### User

```bash
whoami
id
groups
sudo useradd -m username
sudo passwd username
sudo usermod
sudo userdel
sudo userdel -r username
```

### Group

```bash
sudo groupadd groupname
groups username
sudo usermod -aG groupname username
sudo gpasswd -d username groupname
```

### System user information

```bash
cat /etc/passwd
cat /etc/group
sudo cat /etc/shadow
```

### Login information

```bash
who
w
last
```

### Switching

```bash
su - username
sudo -u username command
```

---

# 🎯 Interview Questions

### Q1. What is a Linux user?

A user is an account that identifies and controls access to system resources.

---

### Q2. What is a Linux group?

A group is a collection of users used to manage permissions for multiple users efficiently.

---

### Q3. What is UID?

UID is the numeric identifier assigned to a Linux user.

---

### Q4. What is GID?

GID is the numeric identifier assigned to a Linux group.

---

### Q5. Where are Linux users stored?

Basic user-account information is stored in:

```text
/etc/passwd
```

Password hashes and password-account information are stored in:

```text
/etc/shadow
```

---

### Q6. Where are Linux groups stored?

```text
/etc/group
```

---

### Q7. Difference between `whoami` and `id`?

```text
whoami → displays current username
id     → displays UID, GID and group membership
```

---

### Q8. Difference between `useradd` and `usermod`?

```text
useradd → creates a new user
usermod → modifies an existing user
```

---

### Q9. How do you add a user to a group?

```bash
sudo usermod -aG developers username
```

---

### Q10. Why is `-a` important?

Because `-a` means **append**. Without it, `-G` can replace the user's existing supplementary group memberships.

---

### Q11. How do you delete a user?

```bash
sudo userdel username
```

To also remove the home directory:

```bash
sudo userdel -r username
```

---

### Q12. What is `sudo`?

`sudo` allows an authorized user to execute commands with elevated privileges.

---

### Q13. What is the `root` user?

The root user is the Linux superuser with extensive administrative privileges.

---

### Q14. What is the purpose of groups in DevOps?

Groups make access management easier by allowing permissions to be assigned to a set of users rather than configuring every user individually.

---

# 🧠 Lesson 7 — Must Remember

Before moving ahead, make sure you understand:

```text
☑ User
☑ Root
☑ UID
☑ Group
☑ GID
☑ Primary group
☑ Supplementary group
☑ whoami
☑ id
☑ groups
☑ who
☑ w
☑ last
☑ /etc/passwd
☑ /etc/shadow
☑ /etc/group
☑ useradd
☑ usermod
☑ userdel
☑ passwd
☑ groupadd
☑ su
☑ sudo -u
☑ sudo
☑ wheel
☑ User + Group + Ownership + Permissions
☑ Least privilege
```

## 🔥 The most important concept from Lessons 6 + 7

Think of Linux access like this:

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

Example:

```text
dev1
  ↓
devops group
  ↓
/opt/devops-app
  ↓
group = devops
  ↓
group permissions = rwx
  ↓
dev1 can work with the application
```

This concept will come back repeatedly when you learn **SSH → Web Servers → Docker → Jenkins → Ansible → Kubernetes**.
