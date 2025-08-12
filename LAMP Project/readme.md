# WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS

Deploying web applications in the cloud has become a standard practice for developers and system administrators.  
One of the most popular environments for hosting dynamic websites and applications is the **LAMP stack**, which combines a set of open-source technologies to deliver a complete web server solution.

**LAMP** stands for:

- **Linux** – The operating system  
- **Apache** – The web server (alternatively Nginx)  
- **MySQL** – The relational database management system  
- **PHP** – The server-side scripting language (sometimes replaced by Perl or Python)  

In this guide, we will walk through the **installation, configuration, and deployment** of a LAMP stack on an **Amazon Web Services (AWS) EC2 instance** running **Ubuntu**.  
By the end, you will have a fully functional cloud-based web server capable of serving both static and dynamic content.

## Step 1: Connect to the EC2 Instance

1. Open **Terminal** (Linux/Mac) or **PowerShell** (Windows).  
2. Navigate to the folder containing your AWS key pair (`.pem` file).  
3. Connect to your EC2 instance via SSH:  

ssh -i "your-key.pem" ubuntu@<EC2_PUBLIC_IP>


<img width="1588" height="882" alt="image" src="https://github.com/user-attachments/assets/5d5d02d3-6683-4f05-bc16-41f3e9448394" />



## Step 2:  Update the Server
Update all packages to the latest version:
sudo apt update && sudo apt upgrade -y

<img width="997" height="148" alt="image" src="https://github.com/user-attachments/assets/d2d92d69-88f7-4b9c-97c0-c9d4a1314be5" />



## Step 3: Install Apache
sudo apt install apache2 -y

<img width="1414" height="513" alt="image" src="https://github.com/user-attachments/assets/d454ad96-1073-46a5-b4dd-5d7ab8c6812b" />


Verify Apache installation:

sudo systemctl status apache2


<img width="1221" height="426" alt="image" src="https://github.com/user-attachments/assets/88c50d16-533e-458a-88ff-2b482b220c28" />




## Step 4: Install Mysql
sudo apt install mysql-server -y

<img width="1808" height="278" alt="install mysql" src="https://github.com/user-attachments/assets/9c43905f-aec3-4a74-afea-5960a116d358" />


Secure the MySQL installation

Set root password

Remove anonymous users

Disallow remote root login

<img width="1157" height="213" alt="changing mysql password" src="https://github.com/user-attachments/assets/18c96ff3-50a1-428d-ab16-a97d3b0add9e" />



## Step 5: Install PHP

sudo apt install php libapache2-mod-php php-mysql -y

<img width="1383" height="396" alt="image" src="https://github.com/user-attachments/assets/a5e65d5e-64f3-488f-b369-f31bdaaf5286" />


Verify PHP version:
php -v

<img width="940" height="156" alt="image" src="https://github.com/user-attachments/assets/fbf9fd0a-71ec-4d84-bf97-f319cea109c2" />


## Step 6: Create a Virtual Host
Create a directory for your project:
-mkdir /var/www/projectlamp
Assign ownership to your current user:
-chown -R $USER:$USER /var/www/projectlamp
Create and open a virtual host configuration file:
sudo nano /etc/apache2/sites-available/projectlamp.conf
Add the following configuration:
<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>


<img width="1016" height="426" alt="image" src="https://github.com/user-attachments/assets/86139e16-bd7f-4157-aaa8-ebd3df3d4cb7" />



## Step 7: Enable the Virual host
Enable:sudo a2ensite projectlamp

Disable the default site:

sudo a2dissite 000-default

Reload Apache:

sudo systemctl reload apache2

## Step 8: Test the website
Host your website files:
/var/www/projectlamp/index.html

<img width="875" height="112" alt="image" src="https://github.com/user-attachments/assets/1ebb297c-eb18-42b0-9893-1845e6efc675" />

Visit in your browser:

http://<EC2_PUBLIC_IP>

Webpage: 

<img width="1406" height="584" alt="image" src="https://github.com/user-attachments/assets/056b09e5-f0b8-426b-ac0d-a9fd7388824a" />

## Step 9: Enable PHP on the Website
sudo vim /etc/apache2/mods-enabled/dir.conf

Change:

DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm

To:

DirectoryIndex index.php index.html index.cgi index.pi index.xhtml index.htm

Save and close the file. Reload Apache with this command:

sudo systemctl reload apache2

Create a new file called index.php inside the web root:

vim /var/www/projectlamp/index.php

Add this to the blank file:

<?php
phpinfo();
?>
You should see this:

<img width="1200" height="907" alt="image" src="https://github.com/user-attachments/assets/ca8f82a6-31d0-487b-b59c-470089bea7bb" />










