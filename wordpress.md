
# 📝 WordPress on Apache using Vagrant (Ubuntu 20.04)

🚀 **[Live Demo →]  https://drive.google.com/file/d/1wIdgwPL_iQToa4SPPINRU7vKAe9mbzV_/view?usp=sharing


This project sets up a local WordPress environment using **Vagrant**, **Ubuntu 20.04 (focal64)**, and **Apache HTTP Server**, with a custom virtual host configuration.

---

## ⚙️ Stack

- Vagrant + VirtualBox  
- Ubuntu 20.04 (focal64)  
- Apache HTTP Server  
- PHP, MySQL  
- WordPress (manual install)

---

## 🚀 Setup Steps

### 1. Start the VM
```bash
vagrant up
vagrant ssh
````

### 2. Install LAMP Stack (inside VM)

```bash
sudo apt update
sudo apt install apache2 mysql-server php php-mysql libapache2-mod-php php-cli unzip curl -y
```

### 3. Download and Setup WordPress

```bash
cd /tmp
curl -O https://wordpress.org/latest.tar.gz
tar -xzf latest.tar.gz
sudo mv wordpress /var/www/
```

### 4. Set Permissions

```bash
sudo chown -R www-data:www-data /var/www/wordpress
sudo chmod -R 755 /var/www/wordpress
```

### 5. Configure Apache Virtual Host

```bash
sudo vim /etc/apache2/sites-available/wordpress.conf
```

Paste this:

```apache
<VirtualHost *:80>
    ServerAdmin admin@localhost
    ServerName wordpress.test
    DocumentRoot /var/www/wordpress
    <Directory /var/www/wordpress>
        AllowOverride All
        Require all granted
    </Directory>
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

### 6. Enable Virtual Host

```bash
sudo a2ensite wordpress.conf
sudo a2dissite 000-default.conf
sudo a2enmod rewrite
sudo systemctl reload apache2
```

---

## 🖥️ On Your Host Machine

Edit your **hosts** file:

* On Windows: `C:\Windows\System32\drivers\etc\hosts`
* On Mac/Linux: `/etc/hosts`

Add:

```
192.168.56.10 wordpress.test
```

---

## 🌐 Access

Visit [http://wordpress.test](http://wordpress.test) in your browser to complete the WordPress setup.

---


