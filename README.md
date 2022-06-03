# MY-WORDPRESS-BLOG

HOW TO HOST WORDPRESS BLOG ON MICROSOFT AZURE
Setting up a Wordpress Blog from the scratch


STEPS

• Create a Virtual Machine on Azure (I used Ubuntu server)

• Set up an Nginx server

• set up PHP

• set up Mysql and create a simple database

• Install Wordpress


NOW LETS GO!

I have created my Ubuntu server in Azure. I am using putty as my SSH client.


INSTALL NGINX


First of all, you need to ensure that all the packages on your installation is up to date. Run the following command:

sudo apt update


Now, go ahead to install Nginx. But what is Nginx?


Nginx is the server that will be listening to someone's request to display your website's content. Run the command below to get it installed

sudo apt install nginx


After the installation, check the status of Nginx with command below:

systemctl status nginx


CONFIGURE NGINX

At this point, navigate to the directory where Nginx default configuration file is and do some configuration. Execute the command below:

cd /etc/nginx/

cd sites-available/

ls

vi default


I will explain how I did my configuration below👇

In the ‘listen 80 default server’ it means that the server listens on port 80. In the next line ‘listen [::]:80 default_server’,  the colon you see in the [::] means it can be any ip address but on port 80.
If you want to restrict it to a specific ip address, then add it in the square bracket eg [27.67.240.45]

if your requirement states that it should have SSL/HTTPS, then you have to go to ssl configuration and enable port 443. This is recommended for your security. But I will skip that step for now.


Next step in configuration, you can specify your path if you created a new directory to store your file. I didn’t create a new directory, I am sticking to default path as in  var/www/html


Next thing I added is in this line: ‘index index.html index.htm index.nginx-debian.html index.php.  I only added the index.php  file because I am using PHP for this project. Failure to add will make Nginx not to be able to handle PHP request.


Next line is the server name. It can be your IP address or domain name.  I got my domain name from nip.io website

To add my php script, in the line ‘location ~ /.php$ {‘  means that each time my file has .php in it, it should load the file and pass it to

 fastcgi_pass unix:/var/run/php/php7.4-fpm.sock; to process it.

Save the file and next is to test it. Run the command below:


nginx -t

It shows syntax is okay and it is successful. It is recommended you test your config file to be sure there is no error in it.


Now, restart the server and check the status of your nginx with the command below:

systemctl restart nginx

systemctl status nginx


Test your server

Run the command below

echo "Welcome" > test.html

From the browser, paste IP address or domain name  slash (/) text.html. For instance  34-54-213-67.nip.io/test.html to see if the content of your page is displaying


Displaying? Great!!!


INSTALL PHP

Why does Wordpress require PHP to run?


WordPress uses PHP to do all of its processing including figuring out what the URL’s web request is, what to go grab from the database,
 what files to load and when, validating user permissions, checking options, fetching post metadata, calling plugins, and calling the theme. 
 
It uses it to process form fields, image and document uploads, etc

Before installing PHP, first install the pre-requisites. Run the command below:


sudo apt -y install software-properties-common

sudo add-apt-repository ppa:ondrej/php


Now install PHP using the command follow below. I added an extension because php comes with an extension. It might be to process image or some other things you may need.

apt install -y php7.4 php7.4-fpm php7.4-bcmath php7.4-cli php7.4-common php7.4-curl php7.4-dev php7.4-gd php7.4-intl php7.4-json php7.4-mbstring php7.4-mysql php7.4-opcache php7.4-readline php7.4-xml php7.4-zip


After successful installation, check the PHP version by running the command:

php -v

INSTALL MYSQL SERVER

Why do you need to install Mysql? This is to handle database request. Run the command below:


apt install mysql-server


Installed successfully? good!

Now to ensure our sql server is safe, run the command below:

sudo /usr/bin/mysql_secure_installation


It will require you set up a password. If you experience any error setting up the password, kindly restart or refresh your server and move to the next step.
 If it is a production server, please ensure you set up the password (A strong one) for security.

Lets test our php file to ensure it is working fine. Run the commands below


cd var/www/html

ls -al
I created a simple php config file here.

vi info.php


Now go the browser and paste your domain name or your IP address/info.php eg 23.65.671.40/info.php to test

Is it displaying? good!!!


Now it is time to install Wordpress.


WORDPRESS INSTALLATION

Go to wordpress.org and click on get wordpress.

Copy the download link for use in the terminal

Run the command below

wget https://wordpress.org/latest.zip


After installation, please check using the below:

ls -al

if you check, there is latest.zip which needs to be unzipped, install unzip key

apt install unzip

ls


Move wordpress into var/www/html and then to blog

mv wordpress/ ../

ls

Run the commands below

rm -r blog/

mv wordpress/ blog

ls -al
you can see that website is now  inside the blog directory

cd into blog to see the files

cd blog/

ls

You should see contents in the blog directory


Go your browser to test the wordpress. Example
domainname/blog or ipaddress/blog


You will see your page displaying. GREAT!!!

It will request for database information, so go to your terminal and create database name and password with the command below:


CREATE DATABASE

mysql -u root -p

Enter your password

Then run the command below:

CREATE DATABASE wordpress;

 Note wordpress here is the database name. You can give it any name of your choice.
 
Go to the browser and fill in the database name, username (root) and your password. Every other thing remains the same.

You may experience incorrect password error in the page after you click submit...but no worries.

Go your terminal and run the commands below to confirm your database

show databases;

use mysql;

SELECT * from user;


Note: There is mysql password policy that you have to configure and bring to LOW, Else it will give error

Run the commands below to get that done before setting password

SHOW VARIABLE LIKE 'validate_password';


You will see that, the 'validate_password.policy' is on medium. 

Bring it down to LOW so it can allow you pass through this stage. Run the command below

SET GLOBAL validate_password.policy = LOW;

Check to see if it is now on LOW

SHOW VARIABLE LIKE 'validate_password';

The 'validate_password.policy' should be on LOW now. Correct? good!


Set your password!

ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'Password123#@!';

replace 'Password123#@!'; with your own password and press enter.


Now go your wordpress site again and fill in the password and click submit, it should allow you now. 

It will display the page just as shown below, it simply want you to help run a task :). Right click on the configuration files selected and copy it


Go to your terminal and quit the mysql with the command below:

quit

vi wp-config.php

Paste the config files you copied and save 

Go back to the browser and click on 'Run installation'

You should see a welcome page, kindly fill in the information and click on install wordpress


SUCCESS!! You just installed wordpress. 

CONGRATULATIONS!


You can login and what you will see is dashboard. From there you can make changes and update to your website, just like  the backend. 

Whatever changes you make there will reflect on the URL of blog. Go to domainname/blog to view it. Example 23-56-204-66.nip.io/blog

It is a long step but you did it!!! 








