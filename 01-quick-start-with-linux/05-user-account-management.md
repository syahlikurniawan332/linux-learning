# 05 - User Account Management

## 📖 Introduction

In this lesson, I learned how to manage user accounts in Linux.

The practice covered creating users, assigning passwords, modifying user properties, adding users to groups, locking and unlocking accounts, switching between users, and deleting user accounts.

These are fundamental skills for Linux system administration.

---

## 🎯 Learning Objectives

- Create new Linux users.
- Create users with home directories.
- Set user passwords.
- Modify user home directories.
- Change default user shells.
- Add users to groups.
- Switch between users.
- Lock and unlock user accounts.
- Delete users and their home directories.
- Inspect user information through system files.

---

# 👤 Creating a User

Create a new user named `joker`:

```bash
sudo useradd joker
```

Verify the user in `/etc/passwd`:

```bash
sudo grep -w 'joker' /etc/passwd
```

Output:

```text
joker:x:5001:5001::/home/joker:/bin/sh
```

The entry contains information such as:

```text
Username       : joker
User ID (UID)  : 5001
Group ID (GID) : 5001
Home Directory : /home/joker
Default Shell  : /bin/sh
```

---

## 🐛 Mistake During Practice

I accidentally typed:

```bash
sudi grep -w 'joker' /etc/passwd
```

Output:

```text
zsh: command not found: sudi
```

The correct command was:

```bash
sudo grep -w 'joker' /etc/passwd
```

This reinforced the importance of checking command spelling carefully.

---

# 🏠 Creating a User with a Home Directory

Create another user named `bob` and automatically create their home directory:

```bash
sudo useradd -m bob
```

The `-m` option creates the user's home directory.

Verify it:

```bash
sudo ls -ld /home/bob
```

Output:

```text
drwxr-x--- 2 bob bob 57 Sep 7 10:27 /home/bob
```

The directory is owned by:

```text
User  : bob
Group : bob
```

---

# 🔑 Setting a User Password

Set a password for `joker`:

```bash
sudo passwd joker
```

After entering the new password twice:

```text
passwd: password updated successfully
```

Linux does not display password characters while typing them in the terminal.

Password information is stored securely in:

```text
/etc/shadow
```

rather than directly inside `/etc/passwd`.

---

# 🛠️ Modifying User Properties

The `usermod` command is used to modify an existing user account.

---

## Changing the Home Directory

Change Joker's home directory:

```bash
sudo usermod -d /home/wayne joker
```

Verify the change:

```bash
sudo grep -w 'joker' /etc/passwd
```

Output:

```text
joker:x:5001:5001::/home/wayne:/bin/sh
```

The account now references:

```text
/home/wayne
```

as the user's home directory.

---

# 🐚 Changing the Default Shell

Initially, Joker used:

```text
/bin/sh
```

Change the default shell to Bash:

```bash
sudo usermod -s /bin/bash joker
```

Verify:

```bash
sudo grep -w 'joker' /etc/passwd
```

Output:

```text
joker:x:5001:5001::/home/wayne:/bin/bash
```

The default shell is now:

```text
/bin/bash
```

---

# 👥 Adding a User to a Group

Add `joker` to the `sudo` group:

```bash
sudo usermod -aG sudo joker
```

Verify group membership:

```bash
groups joker
```

Output:

```text
joker : joker sudo
```

The important part of:

```text
-aG
```

is:

```text
-a = append
-G = supplementary groups
```

This adds the user to another group without removing their existing group memberships.

---

# 🔄 Switching Users

Switch from the current user to `joker`:

```bash
su - joker
```

After entering Joker's password, a new login session starts as that user.

Because Joker was added to the `sudo` group, administrative commands can now be executed using:

```bash
sudo <command>
```

For example, during the lab I verified sudo access by reading a root-protected system file.

> Sensitive password hash output from `/etc/shadow` is intentionally not included in this documentation.

Return to the previous user with:

```bash
exit
```

---

# 🔒 Locking a User Account

Temporarily lock Joker's password:

```bash
sudo passwd -l joker
```

Trying to switch to the account:

```bash
su - joker
```

resulted in:

```text
su: Authentication failure
```

This showed that password authentication for the account had been locked.

---

# 🔓 Unlocking a User Account

Unlock the account:

```bash
sudo passwd -u joker
```

After unlocking it:

```bash
su - joker
```

worked again successfully.

---

## 🐛 Duplicate Command During Practice

I accidentally executed:

```bash
sudo passwd -u joker
```

twice.

The second command was unnecessary because the account had already been unlocked.

This was harmless in the lab, but it reinforced the habit of checking command results before repeating administrative actions.

---

# 🗑️ Deleting a User

Delete `bob` together with the user's home directory:

```bash
sudo userdel -r bob
```

The command returned:

```text
userdel: bob mail spool (/var/mail/bob) not found
```

The user itself was still removed successfully.

---

## Verifying User Removal

I later tried deleting `bob` again:

```bash
sudo userdel -r bob
```

Output:

```text
userdel: user 'bob' does not exist
```

This confirmed that the account had already been deleted.

Verify with:

```bash
sudo grep -w 'bob' /etc/passwd
```

No output was returned.

---

## 🐛 Incorrect Home Directory Check

I accidentally checked:

```bash
sudo ls -ld /home/bobo
```

instead of:

```bash
sudo ls -ld /home/bob
```

Output:

```text
ls: cannot access '/home/bobo': No such file or directory
```

This was another reminder to verify paths carefully before executing commands.

---

# 📂 Important Linux User Files

During this lesson, I interacted with two important Linux account files.

### `/etc/passwd`

Contains general user account information such as:

```text
username
UID
GID
home directory
default shell
```

Example:

```text
joker:x:5001:5001::/home/wayne:/bin/bash
```

---

### `/etc/shadow`

Contains protected password-related information for Linux accounts.

Access normally requires elevated privileges.

```bash
sudo cat /etc/shadow
```

Because this file contains sensitive authentication information, its contents should not be exposed unnecessarily.

---

# 💡 Key Takeaways

- `useradd` creates a new user account.
- `useradd -m` creates a user together with a home directory.
- `passwd` sets or changes user passwords.
- `usermod` modifies existing user properties.
- `usermod -d` changes the configured home directory.
- `usermod -s` changes the user's default shell.
- `usermod -aG` adds a user to supplementary groups.
- `groups` displays group membership.
- `su - username` starts a login session as another user.
- `passwd -l` locks password authentication.
- `passwd -u` unlocks it.
- `userdel -r` removes a user and their home directory.
- `/etc/passwd` contains general account information.
- `/etc/shadow` contains protected password information.
- Administrative user management commands commonly require `sudo`.

---

# 🧠 Command Reference

| Command | Purpose |
|---|---|
| `useradd user` | Create a user |
| `useradd -m user` | Create a user with a home directory |
| `passwd user` | Set/change a password |
| `usermod -d` | Change configured home directory |
| `usermod -s` | Change default shell |
| `usermod -aG` | Add user to supplementary group |
| `groups user` | Display user groups |
| `su - user` | Switch to another user |
| `passwd -l` | Lock password authentication |
| `passwd -u` | Unlock password authentication |
| `userdel -r` | Delete user and home directory |
| `grep /etc/passwd` | Inspect user account information |

---

## 📝 Summary

In this lesson, I practiced the complete basic lifecycle of Linux user account management.

I created users using:

```bash
useradd
```

configured passwords using:

```bash
passwd
```

modified account properties using:

```bash
usermod
```

managed group membership, switched between users, temporarily locked and unlocked an account, and finally removed a user using:

```bash
userdel
```

The practice also introduced important Linux account files such as `/etc/passwd` and `/etc/shadow`.

User and group management is an essential Linux administration skill because access to files, applications, administrative commands, and system resources depends heavily on user identity and permissions.