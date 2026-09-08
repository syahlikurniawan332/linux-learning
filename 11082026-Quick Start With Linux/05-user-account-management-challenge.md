# 06 - User Account Management Challenge

## 📖 Introduction

In this challenge, I applied what I learned about Linux user account management.

The challenge involved creating user accounts, setting passwords, modifying account properties, and deleting users with different requirements.

---

## 🎯 Challenge Objectives

- Create Linux user accounts.
- Create users with custom home directory settings.
- Set user passwords.
- Inspect password status.
- Modify user home directories.
- Change default login shells.
- Delete users with and without removing home directories.
- Verify account changes using `/etc/passwd`.

---

# 👤 1. Creating User Accounts

## Create `joker`

```bash
sudo useradd joker
```

Verify the account:

```bash
sudo grep -w 'joker' /etc/passwd
```

Output:

```text
joker:x:5001:5001::/home/joker:/bin/sh
```

This shows that Joker was created with:

```text
Username       : joker
UID            : 5001
GID            : 5001
Home Directory : /home/joker
Shell          : /bin/sh
```

---

## Create `batman`

I created Batman with a home directory using:

```bash
sudo useradd -m batman
```

Verify the directory:

```bash
sudo ls -ld /home/batman
```

Output:

```text
drwxr-x--- 2 batman batman 57 Sep 8 11:44 /home/batman
```

The challenge required Batman to use:

```text
/home/gotham
```

So I changed the configured home directory:

```bash
sudo usermod -d /home/gotham batman
```

Verify:

```bash
sudo grep -w 'batman' /etc/passwd
```

Output:

```text
batman:x:5002:5003::/home/gotham:/bin/sh
```

---

## ⚠️ Important Observation

Changing the configured home directory using:

```bash
usermod -d /home/gotham batman
```

changed the path stored in `/etc/passwd`, but it did not create or move the actual directory.

This became visible later when the system reported:

```text
userdel: batman home directory (/home/gotham) not found
```

So there is an important difference between:

```text
Changing the configured home path
```

and:

```text
Actually creating or moving the user's home directory
```

---

# 🔑 2. Managing User Passwords

Set Joker's password:

```bash
sudo passwd joker
```

Output:

```text
passwd: password updated successfully
```

Set Batman's password:

```bash
sudo passwd batman
```

Output:

```text
passwd: password updated successfully
```

---

## 🐛 Mistake: `passwd -s`

I attempted to check Joker's password status using:

```bash
sudo passwd -s joker
```

Output:

```text
passwd: invalid option -- 's'
```

The command help showed that the correct option is:

```text
-S, --status
```

The correct command should therefore be:

```bash
sudo passwd -S joker
```

and:

```bash
sudo passwd -S batman
```

This reinforced that Linux command options can be case-sensitive.

---

# 🛠️ 3. Modifying User Accounts

## Change Joker's Home Directory

```bash
sudo usermod -d /home/arkham joker
```

Verify:

```bash
grep -w joker /etc/passwd
```

Output:

```text
joker:x:5001:5001::/home/arkham:/bin/sh
```

Joker's configured home directory is now:

```text
/home/arkham
```

---

## Change Batman's Shell

Change Batman's default shell to Bash:

```bash
sudo usermod -s /bin/bash batman
```

Verify:

```bash
grep -w batman /etc/passwd
```

Output:

```text
batman:x:5002:5003::/home/gotham:/bin/bash
```

Batman now uses:

```text
/bin/bash
```

as the default login shell.

---

# 🗑️ 4. Deleting User Accounts

The challenge required two different deletion behaviors:

```text
joker  → delete account only
batman → delete account and home directory
```

---

## Delete Batman and His Home Directory

```bash
sudo userdel -r batman
```

Output:

```text
userdel: batman mail spool (/var/mail/batman) not found
userdel: batman home directory (/home/gotham) not found
```

The account was deleted, but `/home/gotham` did not exist.

This confirmed the earlier observation that changing Batman's configured home directory did not automatically create or move the actual directory.

---

## Delete Joker Without Removing the Home Directory

```bash
sudo userdel joker
```

No `-r` option was used.

This deletes the user account without explicitly requesting removal of the user's home directory.

---

# ✅ Verification

Check Joker:

```bash
grep -w joker /etc/passwd
```

No output was returned.

Check Batman:

```bash
grep -w batman /etc/passwd
```

No output was returned.

Check Gotham directory:

```bash
ls -ld /home/gotham
```

Output:

```text
ls: cannot access '/home/gotham': No such file or directory
```

Both user accounts were successfully removed.

---

# 💡 Key Takeaways

- `useradd` creates a new Linux user.
- `useradd -m` creates the user's home directory.
- `passwd` sets or changes passwords.
- `passwd -S` displays password status.
- Linux command options can be case-sensitive.
- `usermod -d` changes the configured home directory path.
- Changing a home path does not necessarily create or move the directory itself.
- `usermod -s` changes the default login shell.
- `userdel` deletes only the user account.
- `userdel -r` attempts to remove the account together with its home directory and mail spool.
- `/etc/passwd` is useful for verifying account configuration.

---

# 🧠 Command Reference

| Command | Purpose |
|---|---|
| `useradd user` | Create a user |
| `useradd -m user` | Create a user with a home directory |
| `passwd user` | Set or change user password |
| `passwd -S user` | Display password status |
| `usermod -d PATH user` | Change configured home directory |
| `usermod -s SHELL user` | Change default shell |
| `userdel user` | Delete user account |
| `userdel -r user` | Delete user and attempt to remove home directory |
| `grep -w user /etc/passwd` | Verify user account configuration |

---

## 📝 Summary

In this challenge, I practiced managing the lifecycle of Linux user accounts.

I created users, configured passwords, changed home directories and login shells, and deleted accounts with different deletion options.

The challenge also helped me understand two important details.

First, Linux command options are case-sensitive:

```bash
passwd -S
```

is valid, while:

```bash
passwd -s
```

is not.

Second, changing the home directory path with:

```bash
usermod -d
```

changes the account configuration but does not necessarily create or move the physical directory.

These details helped deepen my understanding of how Linux user account configuration relates to the actual filesystem.