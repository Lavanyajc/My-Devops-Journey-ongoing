
# 🚀 Simple Automatic Linux VM Setup using Vagrant (in Git Bash)

This guide helps you quickly set up **CentOS** and **Ubuntu** Virtual Machines automatically using **Vagrant** and **Git Bash** on Windows.



## ✅ Prerequisites

Install these tools before starting:

| Tool          | Download Link                                |
|---------------|-----------------------------------------------|
| Git Bash      | https://git-scm.com                          |
| VirtualBox    | https://www.virtualbox.org                   |
| Vagrant       | https://developer.hashicorp.com/vagrant/downloads |

✅ Check installation in Git Bash:

In git bash
````
vagrant --version
VBoxManage --version
````

---

## 📁 Step-by-Step Setup

### 1️⃣ Create Main Folder in Git Bash

```bash
mkdir vagrant
cd vagrant
mkdir centos
mkdir ubuntu
```

---

### 2️⃣ Setup CentOS VM

```bash
cd centos
vagrant init centos/7        # Official CentOS 7 box from Vagrant Cloud
vagrant up                   # Downloads and boots the CentOS VM
vagrant ssh                  # Access the CentOS VM
```

---

### 3️⃣ Setup Ubuntu VM

```bash
cd ../ubuntu
vagrant init ubuntu/focal64  # Official Ubuntu 20.04 (Focal Fossa) 64-bit box
vagrant up                   # Boots the Ubuntu VM
vagrant ssh                  # Access the Ubuntu VM
```

---

## 📌 Common Vagrant Commands

```bash
vagrant up         # Starts the VM
vagrant ssh        # Logs into the VM
vagrant halt       # Shuts down the VM
vagrant destroy    # Deletes the VM
vagrant status     # Shows current VM state
```





## ✅ Done!

Now your CentOS and Ubuntu VMs are set up with just a few simple steps.

