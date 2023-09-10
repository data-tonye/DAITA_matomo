# Comprehensive Guide to Self-Hosting Matomo Web Analytics on Azure

## 📚 Table of Contents

- [🌟 Introduction](#-introduction)
- [🛠 Prerequisites](#-prerequisites)
- [🚀 Azure Virtual Machine Setup](#-azure-virtual-machine-setup)
- [⚙️ Configuring the Virtual Machine](#-configuring-the-virtual-machine)
- [📦 Database Creation](#-database-creation)
- [🌐 Installing and Configuring Nginx](#-installing-and-configuring-nginx)
- [📈 Download and Extract Matomo](#-download-and-extract-matomo)
- [🔒 SSL Certificate Installation](#-ssl-certificate-installation)
- [🔄 Database Migration: From UVM WebDB to Azure VM](#-Database-Migration:-From-UVM-WebDB-to-Azure-VM)
- [🎯 Completing Matomo Analytics Setup](#-completing-matomo-analytics-setup)

## 🌟 Introduction

Matomo is an open-source web analytics platform that offers 100% data ownership, avoiding data privacy issues commonly associated with third-party analytics services like Google Analytics. This guide aims to provide a step-by-step tutorial for setting up Matomo on an Azure Virtual Machine (VM). 

## 🛠 Prerequisites

- An Azure account
- SSH client (WSL for Windows, terminal for macOS and Linux)

## 🚀 Azure Virtual Machine Setup

### Step 1: Launch Azure Portal
- Open the Azure portal and sign in.
  
### Step 2: Create VM
- Navigate to "Create a resource" > "Compute" > "Virtual Machine."
  
### Step 3: Configure VM
- Fill in the necessary details to configure your VM. Choose 'Linux' as the operating system.

![image](https://github.com/YYinBurgh/DAITA_matomo/assets/69682190/1f7a74ff-527f-4f12-a1cb-57a3135eecb9)

## ⚙️ Configuring the Virtual Machine

### Step 1: SSH into VM
- Open your SSH client and type:

```
ssh username@public_ip_address
```
Replace `username` and `public_ip_address` with your specific details.

### Step 2: Update and Install Packages
- Execute the following commands to update and install essential packages:

```
sudo apt update
sudo apt upgrade -y
sudo apt install php php-cli php-fpm php-curl php-gd mysql-server php-mysql php-xml php-mbstring unzip -y
```

## 📦 Database Creation

### Step 1: Login to MySQL
- Login to MySQL as the root user.

```
sudo mysql -u root -p
```

### Step 2: Create Matomo Database and User
- Execute the following SQL commands:

```
CREATE DATABASE matomo;
CREATE USER 'matomo_user'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON matomo.* TO 'matomo_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

## 🌐 Installing and Configuring Nginx

### Step 1: Install Nginx
- To install Nginx, run:

```
sudo apt install -y nginx
```

### Step 2: Configure Nginx for Matomo
- Create a new Nginx configuration file for Matomo:

```
sudo nano /etc/nginx/sites-available/matomo.conf
```

- Add the server configurations into this file. Make sure to replace `server_name` with your specific domain name.

### Step 3: Activate Configuration
- Link the file to `sites-enabled` and test the configuration:

```
sudo ln -s /etc/nginx/sites-available/matomo.conf /etc/nginx/sites-enabled
sudo nginx -t
sudo systemctl reload nginx.service
```

## 📈 Download and Extract Matomo

### Step 1: Create Web Server Directory
- Create and navigate to the web server directory:

```
sudo mkdir -p /var/www/ && cd /var/www/
```

### Step 2: Download and Extract Matomo
- Run these commands to download and extract Matomo:

```
wget https://builds.matomo.org/matomo.zip
unzip matomo.zip
rm matomo.zip
```

### Step 3: Change Ownership
- Change the ownership of the Matomo directory:

```
sudo chown -R www-data:www-data /var/www/matomo
```

## 🔒 SSL Certificate Installation

### Step 1: Install Certbot
- Install the Certbot repositories and Certbot itself:

```
sudo add-apt-repository ppa:certbot/certbot
sudo apt update
sudo apt install certbot python-certbot-nginx -y
```

### Step 2: Generate SSL Certificate
- Generate the SSL certificate using Certbot:

```
sudo certbot --nginx -d your_domain_name
```

- Now if we look at our /etc/nginx/sites-available/matomo.conf file we should see that certbot has added our SSL configurations for us.
  
## 🎯 Completing Matomo Analytics Setup

- Open your web browser and go to your Matomo installation URL.
- Follow Matomo's on-screen instructions.
![image](https://github.com/YYinBurgh/DAITA_matomo/assets/69682190/014fa600-bf2f-41d7-a044-87b2a1757321)
- When prompted, use the database details you set up earlier.
![image](https://github.com/YYinBurgh/DAITA_matomo/assets/69682190/ab11880d-8a29-448d-813d-69058d1ad1d7)
- Continue going through the configuration and once you get to the `Tracking code` section make sure you copy your tracking code snippet. This is what you will use to add to your website to gather the analytics information.

## 🔄 Database Migration: From UVM WebDB to Azure VM

Database migration can be a crucial task, especially when you're moving from one environment to another. This section will guide you through the process of migrating your existing Matomo database from UVM WebDB to your new Azure VM setup using phpMyAdmin.

- Access to UVM WebDB with phpMyAdmin
- Access to Azure VM's phpMyAdmin
- SSH client for Azure VM

### 🛠 Exporting Database from UVM WebDB

#### Step 1: Log into UVM WebDB phpMyAdmin
- Open your browser and navigate to the UVM WebDB phpMyAdmin URL.
- Login using your UVM WebDB credentials.

#### Step 2: Select the Database
- From the left sidebar, click on the database that you wish to export.

#### Step 3: Export the Database
- Click on the `Export` tab at the top.
- Choose the `Quick` method and the format as `SQL`, then click `Go`.
- Save the exported SQL file on your local machine.

### Importing Database to Azure VM

#### Step 1: SSH into Azure VM
- Open your SSH client and connect to your Azure VM:

```
ssh username@public_ip_address
```

#### Step 2: Install phpMyAdmin on Azure VM
- If phpMyAdmin is not already installed, install it using:

```
sudo apt install phpmyadmin
```

#### Step 3: Access phpMyAdmin on Azure VM
- Open your browser and navigate to your Azure VM phpMyAdmin URL.
- Log in using your Azure MySQL credentials.

#### Step 4: Create a New Database
- Create a new database that will receive the imported data:

```
CREATE DATABASE new_matomo_db;
```

#### Step 5: Import the Database
- Click on the new database you've created from the left sidebar.
- Click on the `Import` tab at the top.
- Choose the exported SQL file and click `Go`.

### Finalizing the Migration

#### Step 1: Update Matomo Configuration
- Open your Matomo `config.ini.php` file:

```
sudo nano /var/www/matomo/config/config.ini.php
```

- Update the database credentials to match those of the new Azure VM database.

#### Step 2: Clear Matomo Cache
- Clear the Matomo cache to apply changes:

```
cd /var/www/matomo/tmp/
sudo rm -rf cache/*
```

#### Step 3: Restart Services
- Restart MySQL and Nginx to ensure all changes take effect:

```
sudo systemctl restart mysql
sudo systemctl restart nginx
```

## 🎉 Conclusion

Congratulations, you've successfully set up a self-hosted Matomo Analytics platform on an Azure VM. You now have full control over your web analytics data, ensuring privacy and security.
![gif](https://i.giphy.com/media/YnSTMd4T9BISZcHcAL/giphy.gif)

Feel free to contribute and raise issues to make this guide even more comprehensive!
