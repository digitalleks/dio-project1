# dio-project1
#Project 1 - LAMP
#This project was created on an EC2 Instance on AWS Cloud.
#After installing Ubuntu on the EC2 instance, and necessary permission was given to access it from the internet, thr instance was accessed via ssh connection.
#For the LAMP Project, the first step was installation of Apache server  as shown below:       which was confirmed working in the fig labeled(Apache-WEb-Test.png).
#update a list of packages in package manager
sudo apt update

#run apache2 package installation
sudo apt install apache2

#To verify that apache2 is running as a Service in our OS, use following command
sudo systemctl status apache2
#output of the status check is shown in file **Apache-install.png**

#Next port 80 was opened on the EC2 instance to allow inbound connections via the web.  Output confirmed in file **Apache-web-Test.png**
http://<Public-IP-Address>:80  #where Public-IP-Address is the public IP of the EC2 instance copied from this command:  
  curl -s http://169.254.169.254/latest/meta-data/public-ipv4

#Next step is the installation of MySql database.
$ sudo apt install mysql-server
$ sudo mysql
#Confirmation of MySql installation is logged in the file **Mysql-install.png**
#After the installation of the Mysql, next is the removal of some insecure default settings and lock down access to your database system.
MYSQL> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
#This is confirmed as shown in file** Mysql-DB-Security.png**
$sudo mysql_secure_installation
  
#Next is the installation of  PHP, the final component in the LAMP stack.
sudo apt install php libapache2-mod-php php-mysql
#Other dependencies were also installed with it: php-mysql & libapache2-mod-php
# Once the installation is finished(logs outpu in **PHP-Install.png**), the following command was run to confirm  PHP version.. Output in file **PHP-Install-COnfirm.png**
$php -v

#FInally, a virtual host was create on the installed website first by creating a new subdirectory inside the default web root folder(/var/www).
$sudo mkdir /var/www/projectlamp
$sudo chown -R $USER:$USER /var/www/projectlamp
$sudo vi /etc/apache2/sites-available/projectlamp.conf
  #edit the projectlamp.conf file with the script below:
  <VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
  #Run the command below to show the new file in the sites-available directory
$sudo ls /etc/apache2/sites-available

  #Enablinng the virtual host
$sudo a2ensite projectlamp
#TO disable the default apache website that was pre-installed
$sudo a2dissite 000-default

#To make sure your configuration file doesn’t contain syntax errors, run:
$sudo apache2ctl configtest

#Finally, reload Apache so these changes take effect:
$sudo systemctl reload apache2

  #To test the new website, open the link in a browser:
  http://<Public-IP-Address>:80
  
#In addition, PHP was enabled on the website with the commands below:
#sudo vim /etc/apache2/mods-enabled/dir.conf
#Edit file with the script below:
<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
#Reload apache for the configuration to take effect
$sudo systemctl reload apache2
#FIle index.php was created  inside the custom web root folder and the script below was copied to it
$vim /var/www/projectlamp/index.php
  <?php
phpinfo();
#The PHP page was tested and output shown in file **PHP-page-Test.png**
#After confirming the working of the PHP information, it was removed to keep the sensitive information safe using the command below:
$sudo rm /var/www/projectlamp/index.php
