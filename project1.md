# Step 0: Creating Pre-Requisites 

Created an AWS account: Here is a screenshot of my AWS homepage: 

![awshomescreen](https://user-images.githubusercontent.com/110178748/182002150-adf19f74-b49e-4678-9a6a-6a63bad4ef0f.png)

# Created a New EC2 Instance called andrewpbl2


![EC2instace](https://user-images.githubusercontent.com/110178748/182002994-c16eaa29-6a4a-4163-b09b-28fd9ccae349.png)


# Connected andrewpbl2 instance using PEM key by running the following command *sh -i andrewpbl1.pem ubuntu@3.86.87.66*


![connecttoec2instance](https://user-images.githubusercontent.com/110178748/182003018-afa69f8c-0ee2-40be-b830-d6eafa29c9c8.png)

# Configuring the EC2 machine to serve a Web server

## Step 1: Installing Apache and Updating the Firewall 

### Installing Apache using apt

update a list of packages in package manager

sudo apt update 

![sudo apt update](https://user-images.githubusercontent.com/110178748/182156847-257add1c-7b05-40ea-bdc1-ba51c67d76f2.png)


run apache2 package installation

sudo apt install apache2

![sudo apt install apache2](https://user-images.githubusercontent.com/110178748/182157547-e656b7bf-4be3-4595-b551-f3bb5a05c637.png)


To verify that apache2 is running as a Service in our OS I ran 

sudo systemctl status apache2

![sudo systemctl status apache2](https://user-images.githubusercontent.com/110178748/182157928-c09a2540-18d5-4672-bfde-003fecd97bf9.png)


opening TCP port 80, the default port for web browsers to access web pages on the Internet

TCP port 22 open by default on my EC2 machine to access it via SSH, so I added a rule to EC2 configuration to open inbound connection through port 80

![inbound rules edit of ec2instance](https://user-images.githubusercontent.com/110178748/182159193-bc29921e-28c5-47c8-889b-61163ea9f1c9.png)


![inbound rules functioning confirmation](https://user-images.githubusercontent.com/110178748/182159374-c40c4f71-d356-42ed-a9d9-bdc70cf8b090.png)


I now Check how I can access it locally in my Ubuntu shell,

TO access my server via DNS name i ran :  curl http://localhost:80  

![1](https://user-images.githubusercontent.com/110178748/182161053-918fc338-b260-4f1d-9f08-d448145ff702.png)
![2](https://user-images.githubusercontent.com/110178748/182161064-16d83799-490f-4bf4-93ef-c527f07fd825.png)
![3](https://user-images.githubusercontent.com/110178748/182161078-5711d9a9-95cc-465e-bfaf-7299d0281b49.png)
![4](https://user-images.githubusercontent.com/110178748/182161085-ec62c066-1782-494b-914b-7a1b4d147345.png)
![5](https://user-images.githubusercontent.com/110178748/182161099-6905e52b-955a-46e9-97f4-0ca58fcb83ca.png)
![6](https://user-images.githubusercontent.com/110178748/182161107-276058dc-6505-47a6-8d44-822a77163c2d.png)
![7](https://user-images.githubusercontent.com/110178748/182161115-b338f32a-1aad-46b7-9507-7147ccf3ad03.png)

**I could also access my server using its ip address with the same result above** using thie following command curl http://127.0.0.1:80

## Test how the Apache HTTP server can respond to requests from the Internet.

Opening web-browser **CHROME** and accesing url for my specific instance case http://3.84.79.60:80
  
![internet request to apacheserver](https://user-images.githubusercontent.com/110178748/182174373-c6975756-b502-4a4d-b9b0-6b48f9c19ed8.png)

Another way to retrieve URL is run command curl -s http://169.254.169.254/latest/meta-data/public-ipv4

![another way to retrieve URL](https://user-images.githubusercontent.com/110178748/182175096-10cfc00b-2800-47de-a6b4-462f75301bd8.png)

# Step: 2 Installing MYSQL, a relational Database Management System

## Installing MYSQL using apt command: $ sudo apt install mysql-server

![install mysql-server](https://user-images.githubusercontent.com/110178748/182176513-8b514f92-e635-45e1-9005-231e01d96aa1.png)

installation is finished I logged in to the MySQL console by typing: sudo mysql 

![logged into mysql-console](https://user-images.githubusercontent.com/110178748/182176843-c25472e8-3dba-47c4-934d-eca49e502c75.png)

I ran security script that comes preinstalled with mysql, this scrip removes insecure defualt setting and locks down access to my 
databse system:  ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';

![security mysql and pwrd](https://user-images.githubusercontent.com/110178748/182177854-0331f0a1-5c86-4e33-86a7-70d3c3b5b229.png)

Started interactive script by running $ sudo mysql_secure_installation

![setting up pword](https://user-images.githubusercontent.com/110178748/182179777-eb602276-3ba6-4825-b04e-82cc91486c19.png)

# Installing PHP

Installing PHP,  the component of my setup that will process code to display dynamic content to the end user. 
In addition to the php package, I need php-mysql, a PHP module that allows PHP to communicate with MySQL-based databases.
I will also need libapache2-mod-php to enable Apache to handle PHP files. 

To install these 3 packages at once, I run: sudo apt install php libapache2-mod-php php-mysql

The Installation is finished, and I ran the following command to confirm my PHP version: php -v

![php version](https://user-images.githubusercontent.com/110178748/182180978-9cadfc9f-f4d9-4ee5-8a84-e1facda11807.png)

My LAMP stack is completely installed and fully operational.

I have loaded 
**Linux (Ubuntu)**
**Apache HTTP Server**
**MySQL**
**PHP**

To test my setup with a PHP script, I will set up a proper Apache Virtual Host to hold my website’s files and folders. 

# Step 4: Creating a virutal host for my website using Apache

Create the directory for "projectlamp" with command:   sudo mkdir /var/www/projectlamp

Next, assign ownership of the directory with your current system user:  sudo chown -R $USER:$USER /var/www/projectlamp

![projectlamp 10](https://user-images.githubusercontent.com/110178748/182192487-eb6ba1d4-1f91-4051-a272-1091923c2440.png)


Then, create and open a new configuration file in Apache’s sites-available directory using your 
preferred command-line editor:  sudo vi /etc/apache2/sites-available/projectlamp.conf

This will create a new blank file. Paste in the following bare-bones configuration by hitting on i on the keyboard to enter the insert mode, and paste the text:

<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

![projectlamp 2](https://user-images.githubusercontent.com/110178748/182183821-4171b0a3-c76d-4931-b2cb-751aff9551e5.png)

To save and close the file, I followed steps below:

1 Hit the esc button on the keyboard
2 Type :
3 Type wq. w for write and q for quit
4 Hit ENTER to save the file

I ran the ls command to show the new file in the sites-available directory:  sudo ls /etc/apache2/sites-available

![projectlamp4](https://user-images.githubusercontent.com/110178748/182184347-2c06abe8-b1ba-4500-98e1-28f278b4d93a.png)


I used the  a2ensite command to enable the new virtual host:

sudo a2ensite projectlamp

![projectlamp5](https://user-images.githubusercontent.com/110178748/182184744-84ba24cf-0e3b-4f3d-a1d3-bb520e68c05d.png)


I Disabled Apache’s default website use a2dissite command:  sudo a2dissite 000-default

![projectlamp7](https://user-images.githubusercontent.com/110178748/182185015-a7518b6d-7c2a-44e1-9dfb-13c56a98cd70.png)

To make sure my configuration file doesn’t contain syntax errors I ran:  sudo apache2ctl configtest

![projectlamp8](https://user-images.githubusercontent.com/110178748/182185228-102ee8fd-6a91-4309-960d-8cd328b394af.png)


Finally I reload Apache so these changes take effect with command:  sudo systemctl reload apache2

![projectlamp 9](https://user-images.githubusercontent.com/110178748/182185515-4d47c9a7-2bf9-4c02-857e-390e455df2e3.png)

My new website is now active, but the web root /var/www/projectlamp is still empty. I Create an index.html file in that location so that I can test that the virtual host works as expected: **sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html**

![projectlamp 11](https://user-images.githubusercontent.com/110178748/182192692-bb272d2b-3944-4975-8c4d-a294020fc9de.png)

I went to my  browser and try to open my website URL using IP address: in the image below I see the text from ‘echo’ command I wrote to index.html file, this  means my Apache virtual host is working as expected.
In the output I see my server’s public hostname (DNS name) and public IP address
**Hello LAMP from hostname ec2-3-84-79-60.compute-1.amazonaws.com with public IP 3.84.79.60**

![projectlamp 12](https://user-images.githubusercontent.com/110178748/182193449-c4fbe3f9-af56-4e21-8e50-fbbbb3cc775e.png)

# Step 5: Enable PHP on my website

With the default DirectoryIndex settings on Apache, a file named index.html will always take precedence over an index.php file. This is useful for setting up maintenance pages in PHP applications, by creating a temporary index.html file containing an informative message to visitors. Because this page will take precedence over the index.php page, it will then become the landing page for the application. Once maintenance is over, the index.html is renamed or removed from the document root, bringing back the regular application page.

In case you want to change this behavior, you’ll need to edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive:

I ran the following command: sudo vim /etc/apache2/mods-enabled/dir.conf

We can see in the image below the default DirectoryIndex settings on apache index.html always takes precedence over index.php file. 

![projectlamp 13](https://user-images.githubusercontent.com/110178748/182197397-d0830ab0-a76f-497c-9176-0bf6d630dc64.png)


I would like to change the default behaviour to this: 

<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>

![projectlamp16](https://user-images.githubusercontent.com/110178748/182205043-3bdfdfb9-e73d-45c1-95f6-7ba8500b4c0a.png)


I reload Apache so the changes take effect:  sudo systemctl reload apache2

![projectlamp17](https://user-images.githubusercontent.com/110178748/182205133-8a9c5ebb-7b58-4c79-b4c1-f62b969da84e.png)


I Create a new file named index.php inside your custom web root folder: vim /var/www/projectlamp/index.php
This will open a blank file. Add the following text, which is valid PHP code, inside the file:
<?php
phpinfo();

![projectlamp18](https://user-images.githubusercontent.com/110178748/182205427-aa72c928-0c0b-4423-ae42-54d2a34dd147.png)


When i am finished, I save and close the file, refresh the page and i see: 

![projectlamp20](https://user-images.githubusercontent.com/110178748/182208625-d990bf87-6d68-4560-ab0e-d47115a14fca.png)












  
  






