
linux/commands/all-linux-commands.md


This file covers:

* ✅ Commands used in each topic
* 📘 Explanations for each command
* 📸 Reference to screenshots taken




# 🐧 Linux Basics: Play 24 to 27 – Command Practice & Explanation



### 🔧 Commands:
```bash
whoami              # Shows the current user
hostname            # Displays the system's hostname
uname -a            # Displays system/kernel info
cat /etc/*release   # Shows OS release/version (CentOS-compatible)
````

### 📘 Explanation:

| Command             | Purpose                                                   |
| ------------------- | --------------------------------------------------------- |
| `whoami`            | Returns the username of the current user                  |
| `hostname`          | Displays the machine's hostname                           |
| `uname -a`          | Shows kernel name, version, system type, and architecture |
| `cat /etc/*release` | Displays Linux distro and version info (CentOS-specific)  |



### 🔧 Commands:

```bash
pwd                 # Prints the current working directory
ls                  # Lists the contents of the directory
ls -l               # Lists contents with permissions, ownership, size
cd ~                # Changes to home directory
cd folder_name      # Navigates into specified folder
cd ..               # Goes one level up
sudo -i             # Switches to root user
exit                # Logs out of root user mode
```

### 📘 Explanation:

| Command   | Purpose                                       |
| --------- | --------------------------------------------- |
| `pwd`     | Tells you the exact directory you are in      |
| `ls`      | Lists files and folders                       |
| `ls -l`   | Shows details: permissions, owner, size, time |
| `cd`      | Changes directories (`~` = home, `..` = back) |
| `sudo -i` | Gains root access (admin privileges)          |
| `exit`    | Exits from root back to regular user          |



### 🔧 Commands:

```bash
mkdir testdir                 # Creates a new directory
cd testdir
touch file1.txt               # Creates an empty file
cp file1.txt copy.txt         # Copies file1 to copy.txt
mv copy.txt renamed.txt       # Renames copy.txt to renamed.txt
rm renamed.txt                # Deletes renamed.txt
cd ..
rmdir testdir                 # Removes the empty directory
```

### 📘 Explanation:

| Command | Purpose                                 |
| ------- | --------------------------------------- |
| `mkdir` | Makes a new directory                   |
| `touch` | Creates a new file                      |
| `cp`    | Copies files from source to destination |
| `mv`    | Moves or renames a file                 |
| `rm`    | Deletes a file                          |
| `rmdir` | Deletes an empty directory              |




### 🔧 Commands:

```bash
vim sample.txt        # Opens or creates a file in Vim
```

🔹 Inside Vim:

| Command   | Mode/Action                  |
| --------- | ---------------------------- |
| `i`       | Enter insert mode to type    |
| `Esc`     | Return to normal mode        |
| `:w`      | Save file                    |
| `:q`      | Quit Vim                     |
| `:wq`     | Save and quit                |
| `:q!`     | Quit without saving          |
| `dd`      | Delete the current line      |
| `yy`      | Copy (yank) the current line |
| `p`       | Paste the copied line        |
| `gg`      | Move to top of file          |
| `Shift+G` | Move to bottom of file       |

📘 Vim is a powerful command-line text editor useful for DevOps tasks, especially for editing config files inside Linux VMs.



