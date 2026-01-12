# Linux User Auditing and Monitoring Commands

## Topic: User Account Auditing and System Login Monitoring

This guide covers essential Linux commands for auditing user accounts, monitoring login activities, and reviewing system access without modifying users or groups.

---

## Table of Contents
1. [Currently Logged-in Users](#currently-logged-in-users)
2. [User Account Information](#user-account-information)
3. [Group Information](#group-information)
4. [Login History and Tracking](#login-history-and-tracking)
5. [User Activity Monitoring](#user-activity-monitoring)
6. [Practical Examples](#practical-examples)

---

## Currently Logged-in Users

### `who`
Displays currently logged-in users.

```bash
who
```

**Output:**
```
sam-user pts/1    2026-01-12 17:48 (192.168.1.10)
ec2-user pts/2    2026-01-12 17:49 (192.168.1.11)
```

**Common Options:**
- `who -a` - All information
- `who -b` - Last system boot time
- `who -u` - Shows idle time
- `who -H` - Display column headers

---

### `whoami`
Displays the current effective username.

```bash
whoami
```

**Output:**
```
root
```

---

### `w`
Shows who is logged in and what they're doing (more detailed than `who`).

```bash
w
```

**Output:**
```
17:50:23 up 15 min, 2 users, load average: 0.00, 0.00, 0.02
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
sam      pts/1    192.168.1.10     17:48    0.00s  0.02s  0.00s w
ec2      pts/2    192.168.1.11     17:49    6.00s  0.02s  0.00s bash
```

**Common Options:**
- `w -h` - Without header
- `w -s` - Short format
- `w -f` - Full format with remote hostname

---

### `users`
Simple list of currently logged-in usernames.

```bash
users
```

**Output:**
```
root sam-user ec2-user
```

---

## User Account Information

### `id`
Shows user ID (UID), group ID (GID), and group memberships.

```bash
id
id username
```

**Output:**
```
uid=1000(sam) gid=1000(sam) groups=1000(sam),4(adm),27(sudo)
```

**Common Options:**
- `id -u` - Display only UID
- `id -g` - Display only primary GID
- `id -un` - Display username instead of UID
- `id -gn` - Display group name instead of GID

---

### `getent passwd`
Query user database from `/etc/passwd` and other sources (NIS, LDAP).

```bash
getent passwd              # All users
getent passwd username     # Specific user
```

**Output:**
```
sam:x:1000:1000:Sam User:/home/sam:/bin/bash
```

**Field Breakdown:**
1. Username
2. Password (x = stored in /etc/shadow)
3. UID
4. GID
5. User description/full name
6. Home directory
7. Login shell

---

### `cat /etc/passwd`
Directly view the user account file.

```bash
cat /etc/passwd
grep username /etc/passwd
```

**Note:** Use `getent` instead of `cat` for better compatibility with network authentication systems.

---

### `finger`
Displays user information (may need to be installed).

```bash
finger username
```

**Output:**
```
Login: sam            Name: Sam User
Directory: /home/sam  Shell: /bin/bash
Last login Mon Jan 12 17:48 (PKT) on pts/1
No mail.
No Plan.
```

**Installation (if needed):**
```bash
# Debian/Ubuntu
sudo apt install finger

# RHEL/CentOS/Amazon Linux
sudo yum install finger
```

---

## Group Information

### `groups`
Shows groups a user belongs to.

```bash
groups              # Your groups
groups username     # Specific user's groups
```

**Output:**
```
sam : sam adm sudo docker
```

---

### `getent group`
Query group database.

```bash
getent group              # All groups
getent group groupname    # Specific group
```

**Output:**
```
sudo:x:27:sam,john,alice
```

**Field Breakdown:**
1. Group name
2. Password (usually x or *)
3. GID
4. Group members (comma-separated)

---

### `cat /etc/group`
Directly view the group file.

```bash
cat /etc/group
grep groupname /etc/group
```

---

## Login History and Tracking

### `last`
Shows login history from `/var/log/wtmp`.

```bash
last                # All recent logins
last username       # Specific user
last -n 10          # Last 10 entries
```

**Output:**
```
reboot   system boot  6.1.159-181.297. Mon Jan 12 17:35   still running
sam      pts/1        192.168.1.10     Mon Jan 12 17:48   still logged in
reboot   system boot  6.1.159-181.297. Sat Jan 10 09:24 - 17:57 (1+08:32)
wtmp begins Sat Jan 10 09:24:56 2026
```

**Common Options:**
- `last -x` - Show shutdown and runlevel changes
- `last -a` - Display hostname in the last column
- `last -i` - Display IP addresses instead of hostnames
- `last -F` - Full login/logout times
- `last -t YYYYMMDDHHMMSS` - Display state at specified time

**Understanding Output:**
- **reboot** - System reboot
- **still running** - Current session
- **still logged in** - User currently logged in
- **(1+08:32)** - Duration: 1 day, 8 hours, 32 minutes

---

### `lastb`
Shows failed login attempts (requires root privileges).

```bash
sudo lastb          # All failed attempts
sudo lastb -n 10    # Last 10 failed attempts
```

**Output:**
```
root     ssh:notty    192.168.1.50     Mon Jan 12 15:22 - 15:22  (00:00)
admin    ssh:notty    192.168.1.50     Mon Jan 12 15:23 - 15:23  (00:00)
```

**Note:** Reads from `/var/log/btmp`

---

### `lastlog`
Shows the last login time for all users.

```bash
lastlog                 # All users
lastlog -u username     # Specific user
```

**Output:**
```
Username         Port     From             Latest
root             pts/0                     Mon Jan 12 17:35:12 +0500 2026
sam              pts/1    192.168.1.10     Mon Jan 12 17:48:01 +0500 2026
ec2-user         pts/2    192.168.1.11     Mon Jan 12 17:49:15 +0500 2026
john                                       **Never logged in**
```

**Common Options:**
- `lastlog -t DAYS` - Logins within the last DAYS days
- `lastlog -b DAYS` - Logins before DAYS days ago
- `lastlog -u UID` - Specific user by UID

---

## User Activity Monitoring

### `who -a`
All information about logged-in users.

```bash
who -a
```

**Output shows:**
- System boot time
- Runlevel
- Login processes
- Dead processes
- Active terminals

---

### `w -f`
Full detailed output of current user activities.

```bash
w -f
```

Shows complete command being executed by each user.

---

### Process Ownership

#### `ps aux | grep username`
View all processes owned by a specific user.

```bash
ps aux | grep sam
```

---

#### `top -u username`
Real-time view of processes for a specific user.

```bash
top -u sam
```

---

### Login Shell Information

```bash
echo $SHELL              # Current shell
cat /etc/shells          # Available shells on system
```

---

## Practical Examples

### Example 1: Audit All Currently Logged-in Users

```bash
# Quick overview
who

# Detailed view with activities
w

# Check what a specific user is doing
w -u sam
```

---

### Example 2: Investigate a Specific User Account

```bash
# Basic info
id sam

# Account details
getent passwd sam

# Group memberships
groups sam

# Last login
lastlog -u sam

# Recent login history
last sam
```

---

### Example 3: Check for Suspicious Login Activity

```bash
# Recent successful logins
last -n 20

# Failed login attempts (requires root)
sudo lastb -n 20

# All users who never logged in
lastlog | grep "Never"
```

---

### Example 4: System Boot and Uptime History

```bash
# Last system boots
last reboot

# Current uptime
uptime

# System boot time
who -b
```

---

### Example 5: Find Users in a Specific Group

```bash
# View group members
getent group sudo

# List all groups for a user
id -Gn sam
```

---

## Important Files for User Auditing

| File | Purpose | Readable by |
|------|---------|-------------|
| `/etc/passwd` | User account information | Everyone |
| `/etc/shadow` | Encrypted passwords | Root only |
| `/etc/group` | Group information | Everyone |
| `/etc/gshadow` | Secure group information | Root only |
| `/var/log/wtmp` | Login history (used by `last`) | Root only |
| `/var/log/btmp` | Failed login attempts (used by `lastb`) | Root only |
| `/var/log/lastlog` | Last login database (used by `lastlog`) | Everyone |

---

## Common Use Cases

### Security Auditing
```bash
# Check for unexpected users
who

# Review failed logins
sudo lastb | head -20

# Find users who haven't logged in
lastlog | grep "Never"
```

### System Monitoring
```bash
# Current system load and users
w

# Check active sessions
who -u
```

### User Investigation
```bash
# Complete user profile
id sam && groups sam && lastlog -u sam && last sam | head -5
```

---

## Tips and Best Practices

1. **Regular Auditing:** Run `who` and `w` regularly to monitor active sessions
2. **Failed Logins:** Check `lastb` frequently for brute force attempts
3. **Never Logged In:** Review `lastlog` for accounts that have never been used
4. **Use `getent`:** Prefer `getent` over `cat` for querying user/group databases
5. **Root Access:** Many audit logs require root privileges for full visibility
6. **Combine Commands:** Use pipes and grep to filter specific information

---

## Additional Resources

- `man who` - Manual page for who command
- `man last` - Manual page for last command
- `man id` - Manual page for id command
- `/usr/share/doc/` - System documentation directory

---

## Notes

- These commands are read-only and safe for system auditing
- No user or system modifications occur
- Some commands require root/sudo privileges for full functionality
- Output format may vary slightly between Linux distributions

---

**Author:** Haider Ali 
**Date:** January 2026  
**Purpose:** Linux Learning Documentation
