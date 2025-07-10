

This project runs a static HTML template (Mini Finance) on an Apache HTTP Server inside a CentOS 9 Stream virtual machine using Vagrant.

# Mini Finance – Apache Web Server using Vagrant (CentOS 9 Stream)

🚀 **For video----[Live Demo →](https://drive.google.com/file/d/1Nm8fDTptUt5wGObOuZFzM-CPdurAH6wT/view?usp=sharing)**

This project runs a static HTML template (Mini Finance) on an Apache HTTP Server inside a CentOS 9 Stream virtual machine using Vagrant.


---

## Tech Stack
- Vagrant
- VirtualBox
- CentOS Stream 9
- Apache HTTP Server
- Static HTML/CSS/JS

---

## Setup Instructions

1. Start the VM:
```bash
vagrant up
vagrant ssh
````

2. Inside the VM, install and start Apache:

```bash
sudo dnf install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
```

3. Copy HTML files to the web root:

```bash
sudo cp -r /vagrant/html/* /var/www/html/
sudo systemctl restart httpd
```

4. Open browser:

```
http://192.168.56.10
```




