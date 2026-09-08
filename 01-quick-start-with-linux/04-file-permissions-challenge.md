# 04 - File Permissions Challenge

## 📖 Introduction

In this challenge, I practiced managing file ownership and permissions in Linux.

The goal was to create a file, change its owner and group, modify its permissions, and verify the result using Linux commands.

---

## 🎯 Challenge Objectives

- Create a new file using `touch`.
- Change file ownership using `chown`.
- Modify file permissions using `chmod`.
- Verify file ownership and permissions using `ls -l`.

---

## 🧪 Practice

### 1. Create the Target File

Create a new file:

```bash
touch target_file
```

I initially typed the project directory incorrectly:

```bash
cd ~/prject && ls
```

Output:

```text
cd: no such file or directory: /home/labex/prject
```

The correct directory was:

```bash
cd ~/project && ls
```

Output:

```text
target_file
```

---

## 2. Change File Ownership

The required ownership was:

```text
Owner : user1
Group : group1
```

At first, I accidentally used `chmod`:

```bash
sudo chmod user1:group1 target_file
```

Output:

```text
chmod: invalid mode: ‘user1:group1’
Try 'chmod --help' for more information.
```

`chmod` is used to modify permissions, not ownership.

The correct command was:

```bash
sudo chown user1:group1 target_file
```

Verify the result:

```bash
ls -l target_file
```

Output:

```text
-rw-rw-r-- 1 user1 group1 0 Sep 7 09:21 target_file
```

The ownership was successfully changed to:

```text
user1 group1
```

---

## 3. Set File Permissions

The required permission was:

```text
-rwxrw----
```

Using numeric permission notation:

```bash
sudo chmod 760 target_file
```

Verify the result:

```bash
ls -l target_file
```

Output:

```text
-rwxrw---- 1 user1 group1 0 Sep 7 09:21 target_file
```

---

## 🔐 Understanding `760`

The numeric permission:

```text
760
```

means:

```text
7 = rwx
6 = rw-
0 = ---
```

So the final permissions are:

```text
Owner  : rwx
Group  : rw-
Others : ---
```

Which matches:

```text
-rwxrw----
```

---

## 🐛 Mistakes During Practice

### Incorrect Directory Name

I typed:

```bash
cd ~/prject
```

instead of:

```bash
cd ~/project
```

This caused:

```text
No such file or directory
```

---

### Using `chmod` Instead of `chown`

I initially tried:

```bash
sudo chmod user1:group1 target_file
```

This failed because `chmod` changes permissions.

The correct command for changing ownership is:

```bash
sudo chown user1:group1 target_file
```

This helped reinforce the difference between:

```text
chown → ownership
chmod → permissions
```

---

## 💡 Key Takeaways

- `touch` creates a new file.
- `chown` changes the owner and group of a file.
- `chmod` changes file permissions.
- `ls -l` displays ownership and permission information.
- `760` gives full permissions to the owner, read/write permissions to the group, and no permissions to others.
- `chown` and `chmod` have different purposes and should not be confused.
- Small typing mistakes in paths can cause `No such file or directory` errors.

---

## 📝 Summary

In this challenge, I successfully created `target_file`, changed its ownership to `user1:group1`, and configured its permissions to:

```text
-rwxrw----
```

The final commands used were:

```bash
touch target_file
sudo chown user1:group1 target_file
sudo chmod 760 target_file
ls -l target_file
```

Final result:

```text
-rwxrw---- 1 user1 group1 0 Sep 7 09:21 target_file
```

This challenge reinforced my understanding of Linux file ownership and permission management using `chown`, `chmod`, `touch`, and `ls`.