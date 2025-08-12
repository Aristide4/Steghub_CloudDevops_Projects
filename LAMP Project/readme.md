WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS

The LAMP stack is a popular open-source web development platform that consists of four main components:

Linux – Operating system

Apache – Web server or 

MySQL – Relational database management system

PHP – Server-side scripting language (sometimes replaced by Perl or Python)

This guide outlines the setup, configuration, and usage of the LAMP stack on an Amazon Web Services (AWS) EC2 instance running Ubuntu.

Step 1: Connect to the EC2 Instance
Open your terminal (Linux/Mac) or PowerShell (Windows).

Navigate to the folder containing your AWS key pair (.pem file).

Connect to your EC2 instance using SSH:

ssh -i "your-key.pem" ubuntu@<public-ip-address>



Step 2: Update the Server
Once connected, run:

sudo apt update && sudo apt upgrade -y
This ensures all packages are up to date.

Step 3: Install Apache
sudo apt install apache2 -y
Verify Apache installation:

Edit
sudo systemctl status apache2
Test in browser:

Go to http://<your-public-ip>

You should see the Apache2 Ubuntu Default Page.

Step 4: Install MySQL

sudo apt install mysql-server -y
Secure MySQL installation:


sudo mysql_secure_installation
Follow prompts to set:

Root password

Remove anonymous users

Disallow remote root login

Remove test database

Step 5: Install PHP
sudo apt install php libapache2-mod-php php-mysql -y
Verify PHP version:

php -v
Step 6: Test PHP
Create a test PHP file in Apache’s root directory:

sudo nano /var/www/html/info.php
Add the following:
Edit
<?php
phpinfo();
?>

In browser, visit:

http://<your-public-ip>/info.php
You should see the PHP Info page.


