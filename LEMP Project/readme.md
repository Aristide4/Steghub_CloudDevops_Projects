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


<img width="1128" height="502" alt="image" src="https://github.com/user-attachments/assets/bfff5a69-2181-4ac0-be4d-08ca39dc0e68" />




---

## 🛠️ Step 2: Install Nginx
sudo apt install nginx -y
Check if it’s running:
systemctl status nginx

<img width="1453" height="350" alt="nginx status" src="https://github.com/user-attachments/assets/3470486e-3952-4e4e-bab1-7b1c69d65f72" />


-----

## 🛠️ Step 3: Install mysql
sudo apt install mysql-server -y

<img width="1808" height="278" alt="install mysql" src="https://github.com/user-attachments/assets/044b8089-542b-4d9e-86fd-bf5ad2875afd" />



Secure MySQL installation:
sudo mysql_secure_installation
Create a database and user:
CREATE DATABASE website_database;
CREATE USER 'josh' IDENTIFIED BY 'Stughub12!';
GRANT ALL PRIVILEGES ON website_database.* TO 'josh'@'localhost';
FLUSH PRIVILEGES;


<img width="885" height="107" alt="create new user and grant him all access on the website database" src="https://github.com/user-attachments/assets/96d779aa-8230-4a06-87b5-ec6e4fd02687" />




---

## 🛠️ Step 4: Install PHP
sudo apt install php-fpm php-mysql -y


<img width="1825" height="395" alt="install php" src="https://github.com/user-attachments/assets/aa3f2f42-60bf-4b98-86f8-5f7e84b703ab" />



---

## 🛠️ Step 5: Configure nginx to use PHP processor

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

<img width="1437" height="85" alt="disable default nginx host" src="https://github.com/user-attachments/assets/e7dc57e4-1500-4941-8575-71457603c4c7" />

8. Host your website file to test if the virtual host works.

<img width="1120" height="77" alt="image" src="https://github.com/user-attachments/assets/dfe4f1a3-6229-41c6-a5db-416d5170d6c2" />


Open the website in a browser using your IP address:

<img width="1796" height="905" alt="image" src="https://github.com/user-attachments/assets/247dbadc-321a-4ee3-8e0e-7b02d496dc70" />



---
## 🛠️ Step 6: Test PHP
sudo vim /var/www/lemp_project/info.php

Then Add: ?php
phpinfo();
?

Visit in browser:
http://your_server_ip/info.php

<img width="1228" height="841" alt="php version" src="https://github.com/user-attachments/assets/04705922-c37b-4f02-a047-c0f6efe170fd" />


---

## Step 7 - Retrieve Data from a MySQL Database Using PHP
Create a new user with the mysql_native_password authentication method to enable PHP to connect to the MySQL database.
First, create a database website_database and a user named Josh.

<img width="885" height="107" alt="create new user and grant him all access on the website database" src="https://github.com/user-attachments/assets/d7b3ab6c-63d1-4b90-a1bd-01cac37edc0d" />

<img width="1138" height="480" alt="image" src="https://github.com/user-attachments/assets/9a0f1a0f-a207-4678-8844-07ca21f2136d" />

Insert some rows of data into the product table.
USE website_database;

CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50),
    price DECIMAL(10,2)
);

INSERT INTO products (name, price) VALUES
('Shoes', 50.00),
('Watch', 100.00),
('Perfume', 70.00),
('Bag', 40.00),
('Laptop', 800.00),
('Headphones', 120.00);

7. To verify that the data has been saved to the table, run:

 <img width="613" height="432" alt="image" src="https://github.com/user-attachments/assets/d54ebe9f-c04a-4e63-88d2-0151ba393267" />
  
Create a PHP Script to Connect to MySQL and Query Data
1. Create a new PHP file in the web root directory

sudo nano /var/www/projectLEMP/item_list.php
This PHP script connects to the MySQL database, retrieves the content of the learn_list table, and displays the results in a list. If there's an issue with the database connection, it will throw an exception.

Copy the code below into the learn_list.php file:

<img width="715" height="582" alt="image" src="https://github.com/user-attachments/assets/43390e39-ef71-4840-bf90-6886d28d1101" />

2. Access this page in the browser using the domain name or public IP address followed by /item_list.php

  <img width="735" height="556" alt="image" src="https://github.com/user-attachments/assets/7ab53c12-9211-4655-9fc1-c1275f909006" />
 

Conclusion:
The LEMP stack offers a powerful platform for hosting and serving web applications. By combining Linux, Nginx, MySQL (or MariaDB), and PHP, developers can build scalable and reliable web solutions.

