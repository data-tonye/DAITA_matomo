# Self-hosted Web Analytics with Matomo and Azure

## Table of Contents
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Creating the Virtual Machine](#creating-the-virtual-machine)
- [Configuring the Virtual Machine](#configuring-the-virtual-machine)
- [Creating a Database](#creating-a-database)
- [Installing Matomo](#installing-matomo)
- [Installing Nginx and Configuring for Matomo](#installing-nginx-and-configuring-for-matomo)
- [Installing a Let's Encrypt Certificate for SSL](#installing-a-lets-encrypt-certificate-for-ssl)
- [Completing the Matomo Analytics Setup](#completing-the-matomo-analytics-setup)

## Introduction
Matomo is an all-in-one premium web analytics platform with the philosophy of 100% data ownership. Simply stated, you own your data, no one else. That means that no abuse of privacy via Google Analytics, Facebook analytics or any other third-party website analytics software.

Privacy has been a growing concern of mine and I'm starting, bit by bit, to take back some control of that and protect my end users from similar abuses. Be the change you want to see, so the saying goes.

## Prerequisites
- An Azure account
- Access to SSH to configure our virtual machine which will be Linux-based (WSL, Linux, macOS, etc)

## Creating the Virtual Machine
1. Log into your Azure account and search for Free services.
  ![image](https://github.com/YYinBurgh/DAITA_matomo/assets/69682190/abdce5f3-bb20-4f0e-b278-1ca7fcd27421)

2. Select the Linux Virtual Machine.
   ![image](https://github.com/YYinBurgh/DAITA_matomo/assets/69682190/fc60505e-eb86-42ce-98a8-fb5f6921cab1)

3. Configure your virtual machine with your unique information.
   ![image](https://github.com/YYinBurgh/DAITA_matomo/assets/69682190/f0dd0608-12d8-4ab8-9a2a-56be65354cc9)

## Configuring the Virtual Machine
SSH into your virtual machine by typing ssh [username]@[public IP address], replacing [username] with the username you specified in the creation of the virtual machine and then replacing [public IP address] with the public IP address of your virtual machine.

Once we're logged in update the virtual machine by running the following commands:
```bash
sudo apt install php php-cli php-fpm php-curl php-gd mysql-server php-mysql php-xml php-mbstring unzip -y
sudo apt update
sudo apt upgrade -y
```
Now that we're updated to the latest version(s) of our virtual machines software we can continue onto the install.


## Creating a Database
Before you can run Matomo we will need to create a database for Matomo to use. Let's sign into our MySQL as our root user without any password.
```
sudo mysql -u root -p
```
Create a MySQL database and user for Matomo:
```
CREATE DATABASE matomo;
CREATE USER `username@example.com` IDENTIFIED BY 'your_secret_password';
GRANT ALL ON matomo.* TO `username@example.com`;
FLUSH PRIVILEGES;
exit
```

## Installing Matomo
1. Install Nginx: `sudo apt install -y nginx`
2. Configure Nginx for Matomo by creating a configuration file.
```
   sudo nano /etc/nginx/sites-available/matomo.conf
```
3. Now we will need to populate the file with our server configurations. Obviously, change the server_name with your specific server name.
```
server {

  listen [::]:443 ssl http2;
  listen 443 ssl http2;
  listen [::]:80;
  listen 80;

  server_name stats.fivethirtyfour.com;
  root /var/www/matomo/;
  index index.php;

  location ~ ^/(index|matomo|piwik|js/index).php {
    include snippets/fastcgi-php.conf;
    fastcgi_param HTTP_PROXY ""; 
    fastcgi_pass unix:/var/run/php/php7.3-fpm.sock; 
  }

  location = /plugins/HeatmapSessionRecording/configs.php {
    include snippets/fastcgi-php.conf;
    fastcgi_param HTTP_PROXY "";
    fastcgi_pass unix:/var/run/php/php7.3-fpm.sock;
  }

  location ~* ^.+\.php$ {
    deny all;
    return 403;
  }

  location / {
    try_files $uri $uri/ =404;
  }

  location ~ /(config|tmp|core|lang) {
    deny all;
    return 403;
  }

  location ~ \.(gif|ico|jpg|png|svg|js|css|htm|html|mp3|mp4|wav|ogg|avi|ttf|eot|woff|woff2|json)$ {
    allow all;
  }

  location ~ /(libs|vendor|plugins|misc/user) {
    deny all;
    return 403;
  }

}
```
4. Now we will need to activate the new matomo.conf configuration by linking the file to the sites-enabled directory.

`sudo ln -s /etc/nginx/sites-available/matomo.conf /etc/nginx/sites-enabled`

4. Test and reload the Nginx configuration.
```
sudo nginx -t
sudo systemctl reload nginx.service
```

## Installing Nginx and Configuring for Matomo
Follow the guide to install and configure Nginx specifically for Matomo.

## Installing a Let's Encrypt Certificate for SSL
Install the required repositories and create a certificate using the Nginx certbot plugin.

## Completing the Matomo Analytics Setup
Go through the Matomo installation process and configure it with the database information you created earlier.

## Conclusion
Congratulations, you now have Matomo Analytics running on an Azure cloud instance!
