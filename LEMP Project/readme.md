# LEMP Stack Implementation on Ubuntu ec2

This project demonstrates how to deploy a **LEMP stack** (Linux, Nginx, MySQL, PHP) on an Ubuntu server ec2.  
The setup is suitable for hosting dynamic websites and web applications.  

---

## 📌 Project Overview

The **LEMP stack** consists of:
- **Linux** – Operating system (Ubuntu)
- **Nginx** – Web server
- **MySQL** – Database management system
- **PHP** – Server-side scripting language

In this project, I deployed a sample website with a MySQL database that stores items and their prices.

---

## ⚙️ Prerequisites

- An Ubuntu Server (20.04 or later recommended)
- A non-root user with `sudo` privileges
- Internet connection

---

## 🛠️ Step 1: Update System

sudo apt update && sudo apt upgrade -y



---

## 🛠️ Step 2: Install Nginx
sudo apt install nginx -y
Check if it’s running:
systemctl status nginx



-----

##🛠️ Step 3: Install MySQL
sudo apt install mysql-server -y



Secure MySQL installation:
sudo mysql_secure_installation
Create a database and user:




CREATE DATABASE website_database;
CREATE USER 'josh' IDENTIFIED BY 'Stughub12!';
GRANT ALL PRIVILEGES ON website_database.* TO 'josh'@'localhost';
FLUSH PRIVILEGES;

---

## 🛠️ Step 4: Install PHP
sudo apt install php-fpm php-mysql -y





---

##🛠️ Step 5: Configure Nginx to Use PHP Processor
sudo vim /etc/nginx/sites-available/lemp_project
server {
    listen 80;
    server_name your_domain_or_ip;

    root /var/www/lemp_project;

    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}
Enable the site:
sudo ln -s /etc/nginx/sites-available/lemp_project /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx



---
##🛠️ Step 6: Test PHP
sudo vim /var/www/lemp_project/info.php
Add:
<?php
phpinfo();
?>



Visit in browser:
http://your_server_ip/info.php



---

##🛠️ Step 7: Create a Table in MySQL
USE example_database;

CREATE TABLE items (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50),
    price DECIMAL(10,2)
);

INSERT INTO items (name, price) VALUES
('Shoes', 50.00),
('Watch', 100.00),
('Perfume', 70.00),
('Bag', 40.00),
('Laptop', 800.00),
('Headphones', 120.00);


