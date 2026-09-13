# 🐧 Lesson 9 — Linux Processes & Services

Processes and services are **one of the most important Linux topics for Cloud/DevOps**.

On an AWS EC2 server, you'll constantly need to answer questions like:

- Is my application running?
- Why is Nginx down?
- Which process is using port `8080`?
- Why is CPU usage high?
- How do I stop a process?
- How do I restart a service?
- How do I make a service start automatically after reboot?
- Where can I find service logs?

This lesson connects directly to **EC2 → Nginx → Java/Tomcat → Node.js → Docker → Jenkins → Kubernetes**.

---

# 1. What is a Process?

A **process is a running instance of a program**.

For example:

```text
Program:
nginx

        ↓ start

Process:
nginx running in memory
```

Another example:

```text
app.py
  ↓
Python interpreter
  ↓
Running process
```

Every running process has a **PID**.

PID = **Process ID**

---

# 2. Program vs Process

This is an important distinction.

### Program

A program is a file/application stored on disk.

Example:

```text
nginx
```

### Process

When that program is executed:

```text
nginx
 ↓
Running process
```

So:

> **Program = code/software**  
> **Process = running instance of that software**

---

# 3. What is PID?

PID = Process ID.

Example:

```text
nginx → PID 1520
```

You can identify the process using:

```bash
ps
```

or:

```bash
ps aux
```

---

# 4. `ps` Command

`ps` = **Process Status**

Basic:

```bash
ps
```

Example:

```text
PID TTY          TIME CMD
1234 pts/0    00:00:00 bash
1567 pts/0    00:00:00 ps
```

It shows processes associated with your current terminal/session.

---

# 5. `ps aux`

One of the most useful Linux commands:

```bash
ps aux
```

It shows processes across the system.

Example:

```text
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.2  ...    ... ?        Ss   ...      ... /sbin/init
root      1520  0.1  1.0  ...    ... ?        S    ...      ... nginx
ec2-user  2000  0.0  0.3  ...    ... pts/0    Ss   ...      ... bash
```

Important columns:

| Column | Meaning |
|---|---|
| `USER` | Process owner |
| `PID` | Process ID |
| `%CPU` | CPU usage |
| `%MEM` | Memory usage |
| `VSZ` | Virtual memory size |
| `RSS` | Resident memory |
| `TTY` | Terminal |
| `STAT` | Process state |
| `START` | Start time |
| `TIME` | CPU time |
| `COMMAND` | Command/program |

---

# 6. Find a Specific Process

Suppose you want to find Nginx:

```bash
ps aux | grep nginx
```

Or:

```bash
ps -ef | grep nginx
```

You'll commonly see the `grep` command itself in the results.

A cleaner approach on many systems is:

```bash
pgrep -a nginx
```

---

# 7. `top` 🔥

`top` is extremely important for troubleshooting.

Run:

```bash
top
```

It displays processes dynamically.

You can monitor:

```text
CPU
Memory
Processes
Load
PID
Users
```

Typical use:

> "The EC2 server is slow. Let's check `top`."

---

# 8. What to Look for in `top`

You may see:

```text
%Cpu
MiB Mem
Load average
PID
%CPU
%MEM
COMMAND
```

If you find:

```text
PID    %CPU
4321   98.5
```

that process is consuming a lot of CPU.

You can investigate:

```bash
ps -p 4321 -f
```

---

# 9. `htop`

`htop` is an interactive alternative to `top`.

Check if available:

```bash
htop
```

If it isn't installed on your system, install it using your package manager.

Amazon Linux:

```bash
sudo yum install htop
```

Ubuntu:

```bash
sudo apt install htop
```

Then:

```bash
htop
```

It provides an easier-to-read interactive process view.

---

# 10. Process States

Processes can have different states.

Common states include:

| State | Meaning |
|---|---|
| `R` | Running / runnable |
| `S` | Sleeping |
| `D` | Uninterruptible sleep |
| `T` | Stopped |
| `Z` | Zombie |

You may see these in:

```bash
ps aux
```

or:

```bash
top
```

---

# 11. Foreground Process

When you execute:

```bash
ping google.com
```

the command normally runs in the foreground.

Your terminal is occupied by that process.

Press:

```text
Ctrl + C
```

to interrupt it.

---

# 12. Background Process

You can run a command in the background using:

```bash
command &
```

Example:

```bash
sleep 100 &
```

You might see:

```text
[1] 2345
```

Here:

```text
2345 = PID
```

Your terminal remains usable.

---

# 13. `jobs`

Check jobs started from your current shell:

```bash
jobs
```

Example:

```text
[1]+ Running    sleep 100 &
```

---

# 14. `fg`

Bring a background job to the foreground:

```bash
fg
```

If multiple jobs exist:

```bash
fg %1
```

---

# 15. `bg`

If a process has been stopped with:

```text
Ctrl + Z
```

you can continue it in the background:

```bash
bg
```

Example:

```text
Command
   ↓
Ctrl + Z
   ↓
Stopped
   ↓
bg
   ↓
Background
```

---

# 16. `Ctrl + C` vs `Ctrl + Z`

Very important:

### Ctrl + C

Usually sends an interrupt signal and asks the process to terminate.

```text
Ctrl + C
→ interrupt
```

### Ctrl + Z

Suspends/stops the foreground job.

```text
Ctrl + Z
→ stop/suspend
```

Then:

```bash
bg
```

can continue it in the background.

---

# 17. Kill a Process

Suppose:

```text
PID = 4321
```

You can request termination:

```bash
kill 4321
```

By default, `kill PID` sends **SIGTERM (15)**.

This is a polite request:

> "Please terminate."

---

# 18. `kill -9`

If a process refuses to terminate:

```bash
kill -9 4321
```

`-9` sends:

```text
SIGKILL
```

This forcefully terminates the process.

### Important

Don't use:

```bash
kill -9
```

as your first choice.

Prefer:

```bash
kill PID
```

and only use SIGKILL when necessary.

Why?

Because a normal termination gives the application a chance to clean up resources.

---

# 19. Common Signals

| Signal | Number | Purpose |
|---|---:|---|
| `SIGHUP` | 1 | Hangup |
| `SIGINT` | 2 | Interrupt |
| `SIGTERM` | 15 | Request termination |
| `SIGKILL` | 9 | Force kill |
| `SIGSTOP` | 19 | Stop process |

You can also write:

```bash
kill -TERM 4321
```

instead of:

```bash
kill 4321
```

---

# 20. `killall`

You can terminate processes by name:

```bash
killall nginx
```

Be careful.

It can affect **multiple processes with that name**.

Use it only when you understand exactly what will be matched.

---

# 21. `pkill`

Another useful command:

```bash
pkill nginx
```

It sends a termination signal to matching processes.

Again, be careful with broad matches.

---

# 22. Parent and Child Processes

Linux processes can have relationships.

Example:

```text
PID 1000
   │
   ├── PID 1001
   ├── PID 1002
   └── PID 1003
```

Here:

```text
1000 = Parent
1001/1002/1003 = Children
```

You can inspect process relationships with:

```bash
pstree
```

or:

```bash
pstree -p
```

---

# 23. PID 1

On modern Linux systems using systemd, the first userspace process is usually:

```text
PID 1
```

and is:

```text
systemd
```

Check:

```bash
ps -p 1 -f
```

You may see:

```text
UID   PID   PPID  CMD
root    1     0  /sbin/init
```

`/sbin/init` may be a symlink to `systemd`.

PID 1 has a special role in starting and managing system services.

---

# 24. What is a Service?

A **service** is a program designed to run in the background and provide functionality.

Examples:

```text
nginx
sshd
docker
jenkins
crond
```

Conceptually:

```text
User
 ↓
Application request
 ↓
Service
 ↓
Process
```

---

# 25. Process vs Service

### Process

A running instance of a program.

### Service

A managed background application that typically runs continuously and provides a system/application function.

For example:

```text
Nginx service
     ↓
Nginx processes
```

---

# 26. `systemctl`

On systemd-based Linux distributions, `systemctl` is the main command for managing services.

Basic syntax:

```bash
systemctl command service
```

---

# 27. Check Service Status

Example:

```bash
sudo systemctl status nginx
```

You may see:

```text
● nginx.service
   Loaded: loaded
   Active: active (running)
```

The most important line:

```text
Active: active (running)
```

means the service is currently running.

---

# 28. Start a Service

```bash
sudo systemctl start nginx
```

Then:

```bash
sudo systemctl status nginx
```

---

# 29. Stop a Service

```bash
sudo systemctl stop nginx
```

Check:

```bash
sudo systemctl status nginx
```

---

# 30. Restart a Service

```bash
sudo systemctl restart nginx
```

Useful after configuration changes.

Example:

```text
Edit Nginx configuration
        ↓
Test configuration
        ↓
Restart/reload Nginx
```

---

# 31. Reload a Service

```bash
sudo systemctl reload nginx
```

Reload asks the service to reread configuration without completely stopping the service, when the service supports reload.

This can reduce disruption.

---

# 32. Start at Boot

Suppose you want Nginx to automatically start when the EC2 instance boots.

Use:

```bash
sudo systemctl enable nginx
```

Check:

```bash
systemctl is-enabled nginx
```

You may get:

```text
enabled
```

---

# 33. Disable Start at Boot

```bash
sudo systemctl disable nginx
```

Check:

```bash
systemctl is-enabled nginx
```

---

# 34. Enable + Start

These are separate operations.

To configure automatic startup:

```bash
sudo systemctl enable nginx
```

To start it **right now**:

```bash
sudo systemctl start nginx
```

You can combine them:

```bash
sudo systemctl enable --now nginx
```

This means:

> Enable Nginx for boot and start it immediately.

---

# 35. Service Status Cheat Sheet

```bash
sudo systemctl status nginx
```

```bash
sudo systemctl start nginx
```

```bash
sudo systemctl stop nginx
```

```bash
sudo systemctl restart nginx
```

```bash
sudo systemctl reload nginx
```

```bash
sudo systemctl enable nginx
```

```bash
sudo systemctl disable nginx
```

```bash
systemctl is-active nginx
```

```bash
systemctl is-enabled nginx
```

These commands are **MUST KNOW**.

---

# 36. `systemctl list-units`

List currently loaded units:

```bash
systemctl list-units
```

To list services:

```bash
systemctl list-units --type=service
```

---

# 37. List Failed Services

Extremely useful for troubleshooting:

```bash
systemctl --failed
```

If something went wrong after reboot:

```bash
systemctl --failed
```

can quickly show failed units.

---

# 38. `journalctl` — Service Logs 🔥

`systemd` uses the journal for system/service logs.

View logs:

```bash
sudo journalctl
```

For Nginx:

```bash
sudo journalctl -u nginx
```

Follow live logs:

```bash
sudo journalctl -u nginx -f
```

This is similar in concept to:

```bash
tail -f logfile
```

---

# 39. Logs Since Boot

Useful command:

```bash
sudo journalctl -b
```

This shows logs from the current boot.

For a specific service:

```bash
sudo journalctl -u nginx -b
```

---

# 40. Recent Service Logs

You can limit logs:

```bash
sudo journalctl -u nginx --since "1 hour ago"
```

This is extremely useful when troubleshooting:

> "Nginx worked earlier but stopped 20 minutes ago."

---

# 41. `systemctl status` + `journalctl`

A very useful troubleshooting combination:

```bash
sudo systemctl status nginx
```

Then:

```bash
sudo journalctl -u nginx -n 50
```

This gives you the service status and recent logs.

---

# 42. Real EC2 Troubleshooting Example 🚨

Imagine your website isn't loading.

You SSH into the server.

### Step 1 — Check Nginx

```bash
sudo systemctl status nginx
```

Suppose:

```text
Active: failed
```

### Step 2 — Check logs

```bash
sudo journalctl -u nginx -n 50
```

You discover a configuration error.

### Step 3 — Test configuration

```bash
sudo nginx -t
```

If successful:

```text
syntax is ok
test is successful
```

### Step 4 — Restart

```bash
sudo systemctl restart nginx
```

### Step 5 — Verify

```bash
sudo systemctl status nginx
```

This is a **real Cloud/DevOps troubleshooting workflow**.

---

# 43. Find Which Process Uses a Port

This is one of the most useful skills.

Suppose your application should run on:

```text
8080
```

Check:

```bash
sudo ss -lntp | grep :8080
```

You may see information identifying the process listening on that port.

Another commonly used command:

```bash
sudo lsof -i :8080
```

If `lsof` isn't installed, install it with the appropriate package manager.

---

# 44. `ss`

`ss` is used to inspect sockets and network connections.

Example:

```bash
ss -tuln
```

Meaning:

```text
-t → TCP
-u → UDP
-l → listening
-n → numeric
```

For listening TCP/UDP sockets with process information:

```bash
sudo ss -tulpn
```

This is extremely useful for server troubleshooting.

---

# 45. Example — Tomcat

Suppose Tomcat should run on:

```text
8080
```

Run:

```bash
sudo ss -lntp | grep :8080
```

If nothing appears:

```text
No process is listening on 8080
```

Possible reasons:

```text
Tomcat isn't running
Tomcat crashed
Wrong port
Application failed to start
```

Check:

```bash
sudo systemctl status tomcat
```

or inspect the relevant process/logs.

---

# 46. CPU Troubleshooting Workflow

Suppose:

> EC2 CPU is 100%.

### Step 1

```bash
top
```

Find the process.

Example:

```text
PID    %CPU
4210   99.8
```

### Step 2

```bash
ps -p 4210 -f
```

Identify the application.

### Step 3

Investigate logs/application behavior.

### Step 4

If necessary, gracefully terminate/restart the application.

Don't immediately run:

```bash
kill -9
```

---

# 47. Memory Troubleshooting

Use:

```bash
free -h
```

Example:

```text
               total   used   free
Mem:            4.0G   3.2G   ...
```

Then:

```bash
top
```

Look at:

```text
%MEM
```

You can identify processes consuming significant memory.

---

# 48. Process Monitoring Workflow

Memorize:

```text
Server problem
     ↓
top
     ↓
Find high CPU/MEM process
     ↓
PID
     ↓
ps -p PID -f
     ↓
Check logs
     ↓
Take corrective action
```

---

# 49. Service Troubleshooting Workflow

Memorize this too:

```text
Service problem
      ↓
systemctl status service
      ↓
journalctl -u service
      ↓
Check configuration
      ↓
Fix issue
      ↓
restart/reload
      ↓
Verify
```

---

# 50. Practical Lab 🔥

Let's combine everything.

## Lab A — Background Process

Run:

```bash
sleep 300 &
```

Check:

```bash
jobs
```

Find it:

```bash
ps aux | grep sleep
```

Better:

```bash
pgrep -a sleep
```

Kill it:

```bash
kill PID
```

Replace `PID` with the actual process ID.

---

# 51. Lab B — Nginx Service

If Nginx isn't installed:

```bash
sudo yum install nginx
```

Start:

```bash
sudo systemctl start nginx
```

Check:

```bash
sudo systemctl status nginx
```

Enable at boot:

```bash
sudo systemctl enable nginx
```

Check:

```bash
systemctl is-enabled nginx
```

---

# 52. Lab C — Stop and Restart

Stop:

```bash
sudo systemctl stop nginx
```

Check:

```bash
sudo systemctl is-active nginx
```

Start:

```bash
sudo systemctl start nginx
```

Restart:

```bash
sudo systemctl restart nginx
```

---

# 53. Lab D — Check Port 80

Run:

```bash
sudo ss -lntp | grep :80
```

If Nginx is listening, you should see a listening socket associated with Nginx.

Then:

```bash
curl http://localhost
```

You should receive the web server response.

---

# 54. Lab E — Service Logs

Run:

```bash
sudo journalctl -u nginx -n 50
```

Then:

```bash
sudo journalctl -u nginx -f
```

Press:

```text
Ctrl + C
```

to stop following the logs.

---

# 55. Very Important: `kill` vs `systemctl stop`

This is an interview favorite.

### `kill`

Used to send a signal to a **specific process**.

```bash
kill 1234
```

### `systemctl stop`

Used to stop a **managed service**.

```bash
sudo systemctl stop nginx
```

Conceptually:

```text
kill
 ↓
Process-level control

systemctl
 ↓
Service-level management
```

For managed services, prefer `systemctl` rather than manually killing the service process.

---

# 56. Very Important: `start` vs `enable`

Another common interview question.

```bash
systemctl start nginx
```

means:

> Start Nginx **now**.

While:

```bash
systemctl enable nginx
```

means:

> Configure Nginx to start automatically during boot.

To do both:

```bash
sudo systemctl enable --now nginx
```

---

# 57. Very Important: `restart` vs `reload`

### Restart

```bash
sudo systemctl restart nginx
```

Stops and starts the service.

### Reload

```bash
sudo systemctl reload nginx
```

Asks the service to reload its configuration without a full stop/start, if supported.

General idea:

```text
reload → less disruptive
restart → full service restart
```

---

# 🎯 Interview Questions

### Q1. What is a process?

A process is a running instance of a program.

---

### Q2. What is PID?

PID stands for Process ID and uniquely identifies a process while it exists.

---

### Q3. How do you list processes?

```bash
ps
```

or:

```bash
ps aux
```

---

### Q4. How do you monitor processes in real time?

```bash
top
```

or:

```bash
htop
```

---

### Q5. How do you find a specific process?

```bash
ps aux | grep nginx
```

or:

```bash
pgrep -a nginx
```

---

### Q6. How do you terminate a process?

```bash
kill PID
```

---

### Q7. What is the difference between SIGTERM and SIGKILL?

```text
SIGTERM → asks process to terminate gracefully
SIGKILL → forcefully terminates the process
```

---

### Q8. What does `kill -9` mean?

It sends SIGKILL, which forcefully terminates the process.

---

### Q9. What is a service?

A service is a background program managed to provide a system/application function, often under systemd.

---

### Q10. What is `systemctl`?

`systemctl` is the primary command-line tool for managing systemd units, especially services.

---

### Q11. How do you start Nginx?

```bash
sudo systemctl start nginx
```

---

### Q12. How do you stop Nginx?

```bash
sudo systemctl stop nginx
```

---

### Q13. How do you restart Nginx?

```bash
sudo systemctl restart nginx
```

---

### Q14. How do you check Nginx status?

```bash
sudo systemctl status nginx
```

---

### Q15. How do you make Nginx start automatically after reboot?

```bash
sudo systemctl enable nginx
```

---

### Q16. Difference between `start` and `enable`?

```text
start  → starts now
enable → starts automatically at boot
```

---

### Q17. Where do you check systemd service logs?

```bash
journalctl
```

For a particular service:

```bash
journalctl -u nginx
```

---

### Q18. How do you find which process is using port 8080?

```bash
sudo ss -lntp | grep :8080
```

or:

```bash
sudo lsof -i :8080
```

---

### Q19. How do you check failed services?

```bash
systemctl --failed
```

---

### Q20. What would you do if an Nginx service fails?

A good interview answer:

> "First I would check `systemctl status nginx`, then inspect `journalctl -u nginx` for errors, validate the configuration using `nginx -t`, fix the issue, restart or reload the service, and verify that it is running and listening on the expected port."

That's a strong **DevOps troubleshooting answer**.

---

# 🧠 Lesson 9 — MUST KNOW

Before moving on, you should be comfortable with:

```text
☑ Process
☑ Program vs Process
☑ PID
☑ ps
☑ ps aux
☑ pgrep
☑ top
☑ htop
☑ Process states
☑ Foreground process
☑ Background process
☑ jobs
☑ fg
☑ bg
☑ Ctrl+C
☑ Ctrl+Z
☑ kill
☑ SIGTERM
☑ SIGKILL
☑ kill -9
☑ killall / pkill
☑ Parent/child processes
☑ PID 1
☑ systemd
☑ Service
☑ systemctl
☑ start
☑ stop
☑ restart
☑ reload
☑ enable
☑ disable
☑ journalctl
☑ systemctl --failed
☑ ss
☑ Finding process by port
☑ CPU troubleshooting
☑ Memory troubleshooting
```

## 🔥 The 3 commands you should remember first

### Process troubleshooting

```bash
ps aux
```

### Live resource monitoring

```bash
top
```

### Service troubleshooting

```bash
sudo systemctl status nginx
```

And remember this DevOps flow:

```text
                LINUX SERVER
                     │
          ┌──────────┴──────────┐
          ↓                     ↓
      PROCESS                 SERVICE
          │                     │
       ps/top              systemctl
          │                     │
       PID                 status/start/stop
          │                     │
        kill                journalctl
          │                     │
          └──────────┬──────────┘
                     ↓
                TROUBLESHOOT
```

