# 04 - File Permissions and Ownership

## 📖 Introduction

In this lesson, I learned how Linux controls access to files and directories using ownership and permissions.

I practiced changing file ownership with `chown`, modifying permissions with `chmod`, using both numeric and symbolic permission notation, and understanding why execute permission is required to run a script.

---

## 🎯 Learning Objectives

- Understand file and directory ownership.
- Change file ownership using `chown`.
- Change ownership recursively.
- Understand Linux file permissions.
- Modify permissions using numeric notation.
- Modify permissions using symbolic notation.
- Understand execute permission on scripts.

---

## 📚 File Ownership

### Create a File

```bash
cd ~/project
touch example.txt
```

Check the file:

```bash
ls
```

Output:

```text
example.txt
```

Display detailed information:

```bash
ls -l example.txt
```

Output:

```text
-rw-rw-r-- 1 labex labex 0 Sep 7 08:36 example.txt
```

The file is currently owned by the user `labex` and group `labex`.

---

## `chown`

The `chown` command changes the owner and group of a file or directory.

```bash
sudo chown root:root example.txt
```

Check the result:

```bash
ls -l example.txt
```

Output:

```text
-rw-rw-r-- 1 root root 0 Sep 7 08:36 example.txt
```

The owner and group have changed from:

```text
labex labex
```

to:

```text
root root
```

---

## Recursive Ownership with `chown -R`

Create a directory structure:

```bash
mkdir -p new-dir/subdir
```

Create two files:

```bash
echo "Hello, World" > new-dir/file1.txt
echo "Another file" > new-dir/file2.txt
```

Inspect the directory recursively:

```bash
ls -lR new-dir
```

Output:

```text
new-dir:
total 8
-rw-rw-r-- 1 labex labex 13 Sep 7 08:45 file1.txt
-rw-rw-r-- 1 labex labex 13 Sep 7 08:45 file2.txt
drwxrwxr-x 2 labex labex 6 Sep 7 08:45 subdir

new-dir/subdir:
total 0
```

Change ownership recursively:

```bash
sudo chown -R root:root new-dir
```

Check the result:

```bash
ls -lR new-dir
```

Output:

```text
new-dir:
total 8
-rw-rw-r-- 1 root root 13 Sep 7 08:45 file1.txt
-rw-rw-r-- 1 root root 13 Sep 7 08:45 file2.txt
drwxrwxr-x 2 root root 6 Sep 7 08:45 subdir

new-dir/subdir:
total 0
```

The `-R` option applies the ownership change recursively to the directory and its contents.

---

# 🔐 File Permissions

Linux permissions are divided into three categories:

```text
User    (u)
Group   (g)
Others  (o)
```

The main permissions are:

```text
r = read
w = write
x = execute
```

For example:

```text
-rw-rw-r--
```

can be divided into:

```text
rw-   rw-   r--
user  group others
```

---

## Numeric Permission Notation

Linux permissions can also be represented using numbers.

```text
r = 4
w = 2
x = 1
```

The values are combined to create permissions.

For example:

```text
7 = 4 + 2 + 1 = rwx
5 = 4 + 1     = r-x
0             = ---
```

---

## `chmod 700`

Check the existing permissions:

```bash
ls -l example.txt
```

Output:

```text
-rw-rw-r-- 1 root root 0 Sep 7 08:36 example.txt
```

Change the permissions:

```bash
sudo chmod 700 example.txt
```

Check again:

```bash
ls -l example.txt
```

Output:

```text
-rwx------ 1 root root 0 Sep 7 08:36 example.txt
```

`700` means:

```text
Owner  : rwx
Group  : ---
Others : ---
```

Only the owner has permission to read, write, and execute the file.

---

## Directory Permissions

Create a directory:

```bash
mkdir ~/test-dir
```

Set its permissions:

```bash
chmod 700 ~/test-dir
```

Check the directory:

```bash
ls -ld ~/test-dir
```

Output:

```text
drwx------ 2 labex labex 6 Sep 7 08:51 /home/labex/test-dir
```

---

## 🐛 Mistake During Practice

I accidentally typed:

```bash
ls -ld ~/test=dir
```

Output:

```text
ls: cannot access '/home/labex/test=dir': No such file or directory
```

The directory was actually named:

```text
test-dir
```

Using the correct name:

```bash
ls -ld ~/test-dir
```

worked successfully.

This reinforced the importance of checking file and directory names carefully.

---

## Recursive Permissions

Change directory permissions recursively:

```bash
chmod -R 755 ~/test-dir
```

Check the directory:

```bash
ls -ld ~/test-dir
```

Output:

```text
drwxr-xr-x 2 labex labex 6 Sep 7 08:51 /home/labex/test-dir
```

`755` means:

```text
Owner  : rwx
Group  : r-x
Others : r-x
```

The `-R` option applies the permission changes recursively.

---

# 🧪 Practice - Executing a Shell Script

Create a simple Bash script:

```bash
cd ~/project
```

```bash
echo '#!/bin/bash' > script.sh
```

Add a command:

```bash
echo 'echo "Hello, World"' >> script.sh
```

View the script:

```bash
cat script.sh
```

Output:

```bash
#!/bin/bash
echo "Hello, World"
```

Check its permissions:

```bash
ls -l script.sh
```

Output:

```text
-rw-rw-r-- 1 labex labex 32 Sep 7 08:56 script.sh
```

At this point, the file does not have execute permission.

Trying to run it:

```bash
./script.sh
```

Output:

```text
zsh: permission denied: ./script.sh
```

---

## Symbolic Permission Notation

Instead of using numeric notation, permissions can also be changed symbolically.

Add execute permission for the file owner:

```bash
chmod u+x script.sh
```

Here:

```text
u = user / owner
+ = add permission
x = execute
```

Check the result:

```bash
ls -l script.sh
```

Output:

```text
-rwxrw-r-- 1 labex labex 32 Sep 7 08:56 script.sh
```

The owner now has execute permission.

Run the script again:

```bash
./script.sh
```

Output:

```text
Hello, World
```

The script can now be executed successfully.

---

## 💡 Numeric vs Symbolic `chmod`

Numeric notation is useful when setting a complete permission configuration.

Example:

```bash
chmod 755 file
```

Symbolic notation is useful when modifying only a specific permission.

Example:

```bash
chmod u+x file
```

Both methods modify permissions, but they are useful in different situations.

---

## ⚠️ Permission Safety

Commands such as:

```bash
sudo chown
sudo chmod
```

can significantly affect access to files and directories.

Incorrect ownership or permissions can prevent applications from accessing files or create security problems.

Always check the target file, directory, and permission values before applying changes.

---

## 💡 Key Takeaways

- `chown` changes file or directory ownership.
- `chown -R` changes ownership recursively.
- `chmod` modifies file and directory permissions.
- Linux permissions are divided between user, group, and others.
- `r`, `w`, and `x` represent read, write, and execute permissions.
- Numeric notation uses values such as `700` and `755`.
- Symbolic notation uses forms such as `u+x`.
- Scripts require execute permission before they can be run directly.
- `ls -l` helps inspect file permissions and ownership.
- `ls -ld` can be used to inspect the directory itself.
- Small filename mistakes can cause `No such file or directory` errors.

---

## 📝 Summary

In this lesson, I practiced managing Linux file ownership and permissions using `chown` and `chmod`.

I changed ownership for individual files and entire directory structures, modified permissions using numeric and symbolic notation, and learned how Linux permissions affect access to files and directories.

The shell script exercise demonstrated the practical importance of execute permission. Without the `x` permission, the script returned a permission denied error. After adding execute permission using:

```bash
chmod u+x script.sh
```

the script ran successfully.

File ownership and permissions are fundamental Linux concepts for controlling access and maintaining system security.