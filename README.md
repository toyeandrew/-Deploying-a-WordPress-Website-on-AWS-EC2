# How-I-deployed-WordPress-on-AWS-EC2
Project: Deploying a WordPress Blog on AWS EC2 using Apache, PHP, and MySQL
#  Deploying a WordPress Website on AWS EC2

# Overview
This project demonstrates how to deploy a **WordPress blog** on an **AWS EC2 instance** using Apache, PHP, and MySQL.  
It’s part of my hands-on AWS DevOps learning journey to build real-world cloud skills.

---

#  Architecture Diagram
![Architecture Diagram](./architecture-diagram.png)

---

# AWS Services Used
- **EC2** – Virtual server for hosting WordPress  
- **Security Groups** – Control inbound/outbound traffic  
- **Elastic IP** – Static IP for persistent web access  
- **IAM** – Secure permissions for managing EC2  
- **VPC** – Default network configuration  
- **RDS (optional)** – Managed MySQL database (alternative to local DB)

---

# Tech Stack
- **OS:** Amazon Linux 2
- **Web Server:** Apache HTTPD
- **App:** WordPress CMS
- **Database:** MySQL / MariaDB
- **Language:** PHP 8.2

---

 Setup Steps

 1️⃣ Launch EC2 Instance
- Choose Amazon Linux 2 (t2.micro – Free Tier)
- Configure security group:
  - SSH (22) – My IP
  - HTTP (80) – Anywhere
  - HTTPS (443) – Anywhere
- Download key pair and connect via SSH:
  ```bash
  ssh -i "my-key.pem" ec2-user@<Public-IP>

2️⃣ Install Apache, PHP, and MySQL
sudo yum update -y
sudo yum install httpd mariadb-server php php-mysqlnd -y
sudo systemctl start httpd
sudo systemctl enable httpd
sudo systemctl start mariadb
sudo systemctl enable mariadb

3️⃣ Create WordPress Database
sudo mysql -u root -p
CREATE DATABASE wordpress;
CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'strongpassword';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;

4️⃣ Install and Configure WordPress
cd /var/www/html
sudo wget https://wordpress.org/latest.tar.gz
sudo tar -xzf latest.tar.gz
sudo mv wordpress/* .
sudo rm -rf wordpress latest.tar.gz
sudo chown -R apache:apache /var/www/html/
sudo chmod -R 755 /var/www/html/
sudo cp wp-config-sample.php wp-config.php
sudo nano wp-config.php

Update the DB settings:
define( 'DB_NAME', 'wordpress' );
define( 'DB_USER', 'wpuser' );
define( 'DB_PASSWORD', 'strongpassword' );
define( 'DB_HOST', 'localhost' );

5️⃣ Restart Apache
sudo systemctl restart httpd

Visit: http://EC2-Public-IP
