
# 🧰 vProfile DevOps Project (Manual Provisioning)

This project demonstrates a real-world DevOps microservices environment using **Vagrant** and **VirtualBox** with **manual provisioning**. It consists of multiple VMs running key backend services required for a Java web application stack.

---

## 📐 Architecture Overview

| VM Name | IP Address    | Hostname | Service          | OS             |
|---------|---------------|----------|------------------|----------------|
| web01   | 192.168.56.11 | web01    | Nginx (Web)      | Ubuntu 22.04   |
| app01   | 192.168.56.12 | app01    | Tomcat + WAR     | CentOS Stream  |
| rmq01   | 192.168.56.13 | rmq01    | RabbitMQ         | CentOS Stream  |
| mc01    | 192.168.56.14 | mc01     | Memcached        | CentOS Stream  |
| db01    | 192.168.56.15 | db01     | MariaDB (MySQL)  | CentOS Stream  |

---

## 🧰 Technologies Used

- **Vagrant**
- **VirtualBox**
- **MariaDB**
- **RabbitMQ**
- **Memcached**
- **Tomcat 10**
- **Nginx**
- **Java 17**
- **Maven 3.9**
- **Shell Commands**

---

## 🛠 Manual Setup Steps

### 🐳 1. VM Setup

```bash
# Install required Vagrant plugin
vagrant plugin install vagrant-hostmanager

# Clone project and navigate
git clone https://github.com/hkhcoder/vprofile-project.git
cd vprofile-project/vagrant/Manual_provisioning_WinMacIntel

# Bring up all VMs (this takes time)
vagrant up
````

---

## ⚙️ Service-by-Service Setup (in this order)

### 1️⃣ MariaDB Setup (db01)

```bash
vagrant ssh db01

# Update system
sudo dnf update -y
sudo dnf install epel-release -y
sudo dnf install git mariadb-server -y
sudo systemctl start mariadb
sudo systemctl enable mariadb

# Secure MariaDB
mysql_secure_installation  # (Use 'admin123' as root password)

# Create DB and user
mysql -u root -padmin123
CREATE DATABASE accounts;
GRANT ALL PRIVILEGES ON accounts.* TO 'admin'@'localhost' IDENTIFIED BY 'admin123';
GRANT ALL PRIVILEGES ON accounts.* TO 'admin'@'%' IDENTIFIED BY 'admin123';
FLUSH PRIVILEGES;

# Import schema
cd /tmp
git clone -b local https://github.com/hkhcoder/vprofile-project.git
cd vprofile-project
mysql -u root -padmin123 accounts < src/main/resources/db_backup.sql
```

---

### 2️⃣ Memcached Setup (mc01)

```bash
vagrant ssh mc01

sudo dnf update -y
sudo dnf install epel-release -y
sudo dnf install memcached -y
sudo systemctl start memcached
sudo systemctl enable memcached

# Allow remote access
sudo sed -i 's/127.0.0.1/0.0.0.0/' /etc/sysconfig/memcached
sudo systemctl restart memcached
```

---

### 3️⃣ RabbitMQ Setup (rmq01)

```bash
vagrant ssh rmq01

sudo dnf update -y
sudo dnf install epel-release wget -y
sudo dnf -y install centos-release-rabbitmq-38
sudo dnf --enablerepo=centos-rabbitmq-38 -y install rabbitmq-server
sudo systemctl enable --now rabbitmq-server

# Allow web access and create user
echo "[{rabbit, [{loopback_users, []}]}]." | sudo tee /etc/rabbitmq/rabbitmq.config
sudo rabbitmqctl add_user test test
sudo rabbitmqctl set_user_tags test administrator
sudo rabbitmqctl set_permissions -p / test ".*" ".*" ".*"
sudo systemctl restart rabbitmq-server
```

---

### 4️⃣ Tomcat + WAR Deployment (app01)

```bash
vagrant ssh app01

# Install Java & Maven
sudo dnf update -y
sudo dnf install epel-release java-17-openjdk java-17-openjdk-devel git wget unzip -y

# Install Tomcat
cd /tmp
wget https://archive.apache.org/dist/tomcat/tomcat-10/v10.1.26/bin/apache-tomcat-10.1.26.tar.gz
tar xzvf apache-tomcat-10.1.26.tar.gz
sudo useradd --home-dir /usr/local/tomcat --shell /sbin/nologin tomcat
sudo cp -r apache-tomcat-10.1.26/* /usr/local/tomcat
sudo chown -R tomcat:tomcat /usr/local/tomcat

# Create systemd service
sudo tee /etc/systemd/system/tomcat.service <<EOF
[Unit]
Description=Tomcat
After=network.target

[Service]
User=tomcat
Group=tomcat
WorkingDirectory=/usr/local/tomcat
Environment=JAVA_HOME=/usr/lib/jvm/jre
Environment=CATALINA_HOME=/usr/local/tomcat
Environment=CATALINA_BASE=/usr/local/tomcat
ExecStart=/usr/local/tomcat/bin/catalina.sh run
ExecStop=/usr/local/tomcat/bin/shutdown.sh
RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl start tomcat
sudo systemctl enable tomcat

# Build and deploy WAR
cd /tmp
git clone -b local https://github.com/hkhcoder/vprofile-project.git
cd vprofile-project
vim src/main/resources/application.properties  # update DB/RabbitMQ hosts if needed
/usr/local/maven3.9/bin/mvn install

sudo systemctl stop tomcat
sudo rm -rf /usr/local/tomcat/webapps/ROOT*
sudo cp target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war
sudo chown -R tomcat:tomcat /usr/local/tomcat/webapps
sudo systemctl start tomcat
```

---

### 5️⃣ Nginx Setup (web01)

```bash
vagrant ssh web01

sudo apt update && sudo apt upgrade -y
sudo apt install nginx -y

# Nginx config
sudo tee /etc/nginx/sites-available/vproapp <<EOF
upstream vproapp {
    server app01:8080;
}

server {
    listen 80;

    location / {
        proxy_pass http://vproapp;
    }
}
EOF

sudo rm -f /etc/nginx/sites-enabled/default
sudo ln -s /etc/nginx/sites-available/vproapp /etc/nginx/sites-enabled/vproapp
sudo systemctl restart nginx
```

---

## 🔍 Common Errors & Fixes

| Issue                         | Fix                                              |
| ----------------------------- | ------------------------------------------------ |
| `502 Bad Gateway`             | Tomcat not running or app01 not reachable        |
| `User not found` / `no users` | DB was not imported; run the SQL file            |
| `Port 2222 already in use`    | Edit `Vagrantfile` to change SSH forwarded ports |
| `Table 'users' doesn't exist` | It's actually `user` table, not `users`          |

---

## ✅ Result

Access your app via:

```
http://192.168.56.11
```

You should see the vProfile login page powered by Tomcat backend and connected to all microservices.

---

## 📌 Credits

* Original Source: [hkhcoder/vprofile-project](https://github.com/hkhcoder/vprofile-project)
* Setup Docs: Manually derived from `VprofileProjectSetupWindowsAndMacIntel.pdf`

```


###for automatic provisioning just go to the path of automatic_provisioning of your OS then run 'vagrant up' command this is take 30-40 mins to bring up all VM's so wait till last then run web01 private ip in browser ,,,there you go!!


