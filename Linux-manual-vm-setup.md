# 🧑‍💻 Manual Linux VM Setup using VirtualBox (CentOS / Ubuntu)

This guide helps you **manually install and configure Linux VMs** (CentOS or Ubuntu) using VirtualBox, including detailed **network and storage** settings.

---

## 📦 Prerequisites

- ✅ [Oracle VirtualBox](https://www.virtualbox.org/) installed
- ✅ ISO files downloaded:
  - [CentOS 7 ISO](https://www.centos.org/download/)
  - [Ubuntu 20.04 ISO](https://ubuntu.com/download/desktop)

---

## 🛠️ Step-by-Step: Create a New Virtual Machine

### 1️⃣ Open VirtualBox → Click **New**

- **Name:** `CentOS7` or `Ubuntu20`
- **Type:** Linux
- **Version:**
  - CentOS → Red Hat (64-bit)
  - Ubuntu → Ubuntu (64-bit)

---

### 2️⃣ Assign Memory (RAM)

- **Min:** 1024 MB (1 GB)
- **Recommended:** 2048 MB (2 GB)

---

### 3️⃣ Create Virtual Hard Disk

- Select: **Create a virtual hard disk now**
- File type: VDI
- Storage: Dynamically allocated
- Size: **Min 10 GB** (Recommended: 20 GB)

---

## 🌐 Configure Network (for Internet Access)

1. Select VM → Click **Settings** → Go to **Network**
2. Adapter 1:
   - **Enable Network Adapter:** ✅
   - **Attached to:** NAT
   - ✅ This allows access to internet

3. Adapter 2 (Optional for SSH or internal networking):
   - Enable → **Attached to:** Host-Only Adapter or Bridged Adapter

---

## 💾 Mount ISO File

1. Go to **Settings → Storage**
2. Under **Controller: IDE** → Click **Empty**
3. Click the CD icon (top right) → Choose a disk file
4. Select your downloaded ISO (`CentOS-7-x86_64.iso` or `ubuntu-20.04.iso`)

---

## 🔥 Start the Installation

Click **Start** → The ISO boots → Follow installation wizard:

### For CentOS:

- Select Language → `Install CentOS`
- Set **Root password**
- Create **non-root user** (e.g., `vagrant`)
- Set Timezone
- In **Installation Destination**:
  - Select the virtual disk
  - Use **Automatic partitioning**

### For Ubuntu:

- Select "Install Ubuntu"
- Use default options (Normal installation)
- Create user, password, machine name
- Select “Erase disk and install Ubuntu”

---

## 🧠 Post Installation Tips

- Once installed, remove ISO:
  - Go to **Settings → Storage** → Remove optical disk from virtual drive
- Reboot VM
- Login with the user you created

---

## 🖧 Enable Copy-Paste (Optional)

- Go to **Devices → Insert Guest Additions CD image**
- Run the installer inside the VM
- Reboot

---

## 🔍 Verify Setup

Run these basic commands in terminal:

```bash
whoami               # Check current user
ip a                 # Check IP address
df -h                # Check disk usage
free -m              # Check memory
uname -a             # System info
