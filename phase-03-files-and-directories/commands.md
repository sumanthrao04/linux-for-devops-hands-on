# Phase 03 — Linux Commands Reference

## 1. pwd — Print Working Directory

**Purpose:** Displays the current working directory.

Syntax:

```bash
pwd
```

Example:

```bash
pwd
```

Output:

```text
/home/sumanth
```

**DevOps use:** Verify the working directory before running deployment commands.

---

## 2. whoami — Current User Identification

**Purpose:** Displays the effective username of the current process.

```bash
whoami
```

Example output:

```text
sumanth
```

**DevOps use:** Check which user is executing a script or deployment command.

---

## 3. id — User and Group Identification

**Purpose:** Displays UID, GID, and group memberships.

```bash
id
```

Example:

```text
uid=1000(sumanth) gid=1000(sumanth) groups=1000(sumanth)
```

Check another user:

```bash
id devops01
```

Check only UID:

```bash
id -u
```

Check only primary GID:

```bash
id -g
```

Check all group IDs:

```bash
id -G
```

Display group names:

```bash
id -Gn
```

Options:

| Option | Meaning |
|---|---|
| -u | User ID |
| -g | Primary group ID |
| -G | All group IDs |
| -n | Display names instead of numbers |

---

## 4. groups — Group Membership

**Purpose:** Displays the groups a user belongs to.

```bash
groups
```

Check another user:

```bash
groups devops01
```

Example:

```text
devops01 : devops01 appteam
```

**DevOps use:** Verify whether a deployment user belongs to the required application group.

---

## 5. getent — Get Entries from System Databases

**Purpose:** Retrieves information from configured system databases.

Check a user:

```bash
getent passwd devops01
```

Example:

```text
devops01:x:1001:1001::/home/devops01:/bin/bash
```

Fields:

```text
username:password-placeholder:UID:GID:comment:home:shell
```

Check a group:

```bash
getent group appteam
```

**DevOps use:** Check users and groups, including those provided through supported centralized identity services.

---

## 6. useradd — Create Linux Users

**Purpose:** Creates a user account.

```bash
sudo useradd -m devops01
```

Options:

| Option | Meaning |
|---|---|
| -m | Create home directory |
| -d | Specify home directory |
| -s | Specify login shell |
| -u | Specify UID |
| -g | Specify primary group |
| -G | Specify supplementary groups |
| -c | Set account comment |

Example with home directory and shell:

```bash
sudo useradd -m -s /bin/bash developer01
```

Example with a comment:

```bash
sudo useradd -m -c "Development Team User" tester01
```

Verify:

```bash
id developer01
```

---

## 7. passwd — Manage User Passwords

**Purpose:** Changes or manages account passwords.

Set password:

```bash
sudo passwd devops01
```

Lock password authentication:

```bash
sudo passwd -l devops01
```

Unlock password authentication:

```bash
sudo passwd -u devops01
```

Important: Locking a password does not necessarily disable every possible login method, such as SSH public-key authentication.

---

## 8. usermod — Modify Linux Users

**Purpose:** Changes an existing user account.

Add a supplementary group:

```bash
sudo usermod -aG appteam devops01
```

Options:

| Option | Meaning |
|---|---|
| -a | Append supplementary groups |
| -G | Set supplementary group list |
| -g | Change primary group |
| -s | Change login shell |
| -d | Change home directory setting |
| -m | Move home contents when used with -d |
| -c | Change account comment |
| -L | Lock password |
| -U | Unlock password |

Example:

```bash
sudo usermod -c "DevOps Engineer" devops01
```

Change shell:

```bash
sudo usermod -s /bin/bash devops01
```

**Important:** Using `-G` without `-a` replaces the supplementary group list.

---

## 9. userdel — Delete Linux Users

**Purpose:** Removes a user account.

```bash
sudo userdel tester01
```

Remove account and home directory:

```bash
sudo userdel -r tester01
```

`-r` attempts to remove the home directory and mail spool.

**Warning:** Practice only with temporary lab users. Verify that the account is not needed before deleting it.

---

## 10. groupadd — Create Groups

**Purpose:** Creates a Linux group.

```bash
sudo groupadd appteam
```

Verify:

```bash
getent group appteam
```

Example output:

```text
appteam:x:1002:
```

Specify a GID:

```bash
sudo groupadd -g 1500 testgroup
```

The specified GID must be available.

---

## 11. groupmod — Modify Groups

**Purpose:** Changes an existing group.

Rename a group:

```bash
sudo groupmod -n newteam testgroup
```

Change GID:

```bash
sudo groupmod -g 1600 newteam
```

Options:

| Option | Meaning |
|---|---|
| -n | New group name |
| -g | New GID |

Changing a group's GID may require checking existing files for ownership consistency.

---

## 12. gpasswd — Manage Group Membership

**Purpose:** Administers group membership and related group settings.

Add a user:

```bash
sudo gpasswd -a devops01 appteam
```

Remove a user:

```bash
sudo gpasswd -d devops01 appteam
```

Options:

| Option | Meaning |
|---|---|
| -a | Add user |
| -d | Delete user from group |

---

## 13. groupdel — Delete Groups

**Purpose:** Removes a group.

```bash
sudo groupdel newteam
```

A group generally cannot be removed while it is the primary group of an existing user.

---

## 14. ls -l — Display File Permissions and Ownership

**Purpose:** Displays detailed file information.

```bash
ls -l
```

Example:

```text
-rw-r--r--. 1 sumanth appteam 120 Oct 19 10:00 config.txt
```

Fields:

1. File type and permissions
2. Hard link count
3. Owner
4. Group
5. File size
6. Modification time
7. Filename

Useful variations:

```bash
ls -la
```

Shows hidden files.

```bash
ls -lh
```

Displays human-readable sizes.

```bash
ls -ld /home/sumanth
```

Displays the directory's own information rather than listing its contents.

---

## 15. chown — Change File Ownership

**Full form:** Change Owner

**Purpose:** Changes the owning user and optionally group.

Syntax:

```bash
sudo chown USER FILE
```

Example:

```bash
sudo chown devops01 config.txt
```

Change both owner and group:

```bash
sudo chown devops01:appteam config.txt
```

Recursive ownership change:

```bash
sudo chown -R devops01:appteam ./lab-directory
```

`-R` means recursive.

**Warning:** Always verify the target before using `-R`. Do not recursively change ownership of system directories.

---

## 16. chgrp — Change Group Ownership

**Full form:** Change Group

**Purpose:** Changes the owning group of a file or directory.

```bash
sudo chgrp appteam config.txt
```

Recursive example:

```bash
sudo chgrp -R appteam ./lab-directory
```

Verify:

```bash
ls -l config.txt
```

---

## 17. chmod — Change File Permissions

**Full form:** Change Mode

**Purpose:** Modifies file and directory permission bits.

Numeric permission values:

```text
Read    = 4
Write   = 2
Execute = 1
```

### chmod 644

```bash
chmod 644 config.txt
```

Meaning:

```text
Owner  = rw-
Group  = r--
Others = r--
```

### chmod 755

```bash
chmod 755 deploy.sh
```

Meaning:

```text
Owner  = rwx
Group  = r-x
Others = r-x
```

### chmod 600

```bash
chmod 600 private.txt
```

Meaning:

```text
Owner  = rw-
Group  = ---
Others = ---
```

### chmod 750

```bash
chmod 750 deploy.sh
```

Meaning:

```text
Owner  = rwx
Group  = r-x
Others = ---
```

### Symbolic chmod

Add execute permission to owner:

```bash
chmod u+x deploy.sh
```

Remove write permission from group:

```bash
chmod g-w config.txt
```

Remove all permissions from others:

```bash
chmod o-rwx private.txt
```

Assign read/write permissions to owner:

```bash
chmod u=rw private.txt
```

Symbol reference:

| Symbol | Meaning |
|---|---|
| u | User/owner |
| g | Group |
| o | Others |
| a | All |
| + | Add |
| - | Remove |
| = | Set exact permissions |

---

## 18. umask — Default Permission Mask

**Purpose:** Controls permission bits removed when creating files and directories.

Check current mask:

```bash
umask
```

Example:

```text
0022
```

Set temporarily:

```bash
umask 027
```

Create files:

```bash
touch test.txt
mkdir testdir
```

Check:

```bash
ls -ld test.txt testdir
```

Typical results:

```text
File      640
Directory 750
```

A umask of `027` removes group write and all permissions for others from the usual creation modes.

Restore your previous umask after practicing.

---

## 19. stat — File Metadata

**Purpose:** Displays detailed file metadata.

```bash
stat config.txt
```

Important fields:

- Size
- Inode
- Owner
- Group
- Permissions
- Access time
- Modification time
- Metadata change time

Display only numeric permission mode:

```bash
stat -c "%a" config.txt
```

Display owner and group:

```bash
stat -c "%U:%G" config.txt
```

Display permissions, owner, and group:

```bash
stat -c "%a %U %G" config.txt
```

---

## 20. namei — Inspect Directory Path Permissions

**Purpose:** Shows the components of a filesystem path.

```bash
namei -l /srv/devops-phase03-app/config/application.properties
```

The `-l` option displays detailed ownership and permissions for each path component.

**DevOps use:** Troubleshoot a file that appears readable but cannot be accessed because a parent directory lacks execute permission.

---

## 21. getfacl — Get File Access Control Lists

**Purpose:** Displays ACL entries.

```bash
getfacl config.txt
```

ACLs allow more specific access rules than basic owner/group/other permissions.

---

## 22. setfacl — Set File Access Control Lists

**Purpose:** Modifies ACL entries.

Grant read access:

```bash
setfacl -m u:devops01:r-- config.txt
```

Explanation:

| Part | Meaning |
|---|---|
| -m | Modify ACL |
| u | User ACL entry |
| devops01 | Username |
| r-- | Read-only access |

Grant read/write:

```bash
setfacl -m u:devops01:rw- config.txt
```

Remove a user ACL:

```bash
setfacl -x u:devops01 config.txt
```

Verify:

```bash
getfacl config.txt
```

The user also needs appropriate directory traversal permissions.

---

## 23. sudo — Execute with Authorized Privileges

**Full form:** Superuser Do (commonly used explanation)

**Purpose:** Executes commands as another user, normally root, when authorized.

Check effective user:

```bash
sudo whoami
```

Check sudo privileges:

```bash
sudo -l
```

Validate cached credentials or authenticate:

```bash
sudo -v
```

Execute a command as another user:

```bash
sudo -u devops01 whoami
```

Example output:

```text
devops01
```

**DevOps use:** Execute approved administrative commands without directly logging in as root.

---

## 24. visudo — Safely Edit sudo Configuration

**Purpose:** Opens sudoers configuration with syntax checking.

```bash
sudo visudo
```

Validate sudoers syntax:

```bash
sudo visudo -c
```

**Important:** Avoid editing `/etc/sudoers` directly. Incorrect sudoers syntax can disrupt administrative access.

---

## 25. su — Switch User

**Purpose:** Starts a shell or executes commands with another user's identity, subject to authentication and authorization.

Switch to another user:

```bash
su - devops01
```

The `-` starts a login-style session with the target user's environment.

Return to your previous shell:

```bash
exit
```

Compare:

```bash
sudo -u devops01 whoami
```

This runs one command as the specified user rather than opening a persistent shell.

---

## 26. Special Permission Bits

### SUID — Set User ID

Inspect a common SUID executable:

```bash
ls -l /usr/bin/passwd
```

Example:

```text
-rwsr-xr-x
```

The `s` in the owner execute position indicates SUID with execute permission.

### SGID — Set Group ID

Set SGID on a lab directory:

```bash
chmod 2770 ./shared-directory
```

For directories, SGID commonly causes newly created entries to inherit the directory's group.

### Sticky Bit

Set sticky bit on a shared lab directory:

```bash
chmod 1777 ./sticky-directory
```

Inspect `/tmp`:

```bash
ls -ld /tmp
```

Example:

```text
drwxrwxrwt
```

The final `t` indicates the sticky bit.

Use special permissions carefully because they affect security and access behavior.

---

## 27. Production Troubleshooting Commands

### Scenario — Jenkins Deployment Permission Denied

Error:

```text
Permission denied: /opt/myapp/config/application.properties
```

Identify current user:

```bash
whoami
```

Check group membership:

```bash
id
```

Inspect file ownership:

```bash
ls -l /opt/myapp/config/application.properties
```

Inspect directory path:

```bash
namei -l /opt/myapp/config/application.properties
```

Inspect detailed metadata:

```bash
stat /opt/myapp/config/application.properties
```

Inspect ACL:

```bash
getfacl /opt/myapp/config/application.properties
```

On CentOS Stream 9, SELinux may also affect access. Inspect relevant SELinux contexts and audit records if traditional permissions appear correct.

**Troubleshooting principle:** Identify the actual access restriction before changing permissions or ownership.

---

## 28. Quick Command Reference

| Command | Purpose |
|---|---|
| `pwd` | Print working directory |
| `whoami` | Show effective username |
| `id` | Show user and group IDs |
| `groups` | Show group memberships |
| `getent` | Query system databases |
| `useradd` | Create user |
| `passwd` | Manage password |
| `usermod` | Modify user |
| `userdel` | Delete user |
| `groupadd` | Create group |
| `groupmod` | Modify group |
| `gpasswd` | Manage group membership |
| `groupdel` | Delete group |
| `ls -l` | Show file permissions and ownership |
| `chown` | Change owner |
| `chgrp` | Change group |
| `chmod` | Change permissions |
| `umask` | Control default permission mask |
| `stat` | Display file metadata |
| `namei` | Inspect path components |
| `getfacl` | Display ACL |
| `setfacl` | Modify ACL |
| `sudo` | Run authorized privileged commands |
| `visudo` | Edit/validate sudoers |
| `su` | Switch user |
| `exit` | Exit shell/session |

## Learning Outcome

After practicing these commands, I can:

- Create and manage Linux users
- Manage groups and memberships
- Understand UID and GID
- Change file ownership
- Configure file and directory permissions
- Understand numeric and symbolic chmod
- Manage ACLs
- Inspect sudo privileges
- Troubleshoot common permission-denied errors
- Apply least-privilege principles in Linux administration

**Next Phase:** Linux Text Processing — grep, awk, sed, cut, sort, uniq, wc, and production log analysis.