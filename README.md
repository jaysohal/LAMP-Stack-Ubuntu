# LAMP-Stack-Linux-Ubuntu (Linux, Apache, MYSQL, PHP)

The purpose of this guide is to show how to install LAMP stack on Ubuntu. It is a group of open source softwares installed together that enables a linux server to host dynamic websites and webapps written in PHP. 

Prerequisite: You already have a running Ubuntu Linux machine running locally (example: VMWare) or on a cloud server (example: Amazon Web Services)

###### Note: We will be using ```sudo``` command that grants super user privilleges to the user and allows you to download packages. 

# Table of Contents
1. [Install Apache](1)
2. [Install MY SQL](1)
3. [Install PHP](1)
4. [Update Firewall Rules](1)
5. [Setup Virual Webhost](1)
6. [Test PHP Processing](1)
7. [Test Database Connection](1)

## 1. Install Apache

Start by updating the local package index to provide the system with most recent information available about the packages from the repositories. 

```
sudo apt update
```

Then install Apache with

```
sudo apt install apache2
```

## 2. Install MYSQL

Installing the database that will house and manage the data on your website is necessary now that the Apache web server is up and running. One of the most popular database management system for PHP environments is MySQL. Start by installing this with 

```
sudo apt install mysql-server
```

## 3. Install PHP

You that you have Apache to server your data and MySQL to store and manage your data. PHP will allow you to process code to display dynamic content to the final user. Install PHP with 

```
sudo apt install php libapache2-mod-php-mysql
```

## 4. Update Firewall Rules

Once the installation is finished, you will need to adjust your firewall settings to allow HTTP traffic. Ubuntu's default firewall configuration tool is called Uncomplicated Firewall (UFW). List all the currently available UFW application profiles with

```
sudo ufw app list
```

To allow traffic on port 80, use the Apache profile

```
sudo ufw allow in "Apache"
```

Verify the change with

```
sudo ufw status
```

To confirm that everything went smoothly, visit your server's public IP address in your web browser. http://```your_server_ip```

## 5. Create Virtual Host For Your Website

When using Apache, you can create virtual hosts which divides the web server into seperate blocks to encapsulate configuration details and host more than one domain from a single server. 

Start by creating a directory for your domain

```
sudo mkdir /var/www/my_domain
```

Now assign the ownership of that directory with the ```$USER``` environment variable, which will reference your current system user

```
sudo chown -R $USER:$USER /var/www/my_domain
```

Then open a new configuration file in Apache's ```sites-available``` directory using ```nano``` command

```
sudo nano /etc/apache2/sites-available/my_domain.conf
```

The above command will create a blank file. Next step is to add the following configuration for your domain

```
<VirtualHost *:80>
    ServerName my_domain
    ServerAlias www.my_domain
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/my_domain
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Save and close the file when you are done by using ```ctrl+X``` then ```Y``` then ```Enter```

Now, we will enable the new virtual host that was just created

```
sudo a2ensite my_domain
```

Now reload the Apache server with

```
sudo systemctl reload apache2
```
