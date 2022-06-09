# dio-project1
## Project 1 - LAMP
This project was created on an EC2 Instance on AWS Cloud.
After installing Ubuntu on the EC2 instance, and necessary permission was given to access it from the internet, thr instance was accessed via ssh connection.
For the LAMP Project, the first step was installation of Apache server  as shown below:       which was confirmed working in the fig labeled
[APache Install](https://user-images.githubusercontent.com/61512079/172871711-491799d0-b516-4dea-824d-f585028848a7.PNG "Apache Install Confirm")

update a list of packages in package manager
```bash
$sudo apt update
```
Run apache2 package installation
```bash
$sudo apt install apache2
```
To verify that apache2 is running as a Service in our OS, use following command
```bash
$sudo systemctl status apache2
```
Output of the status check is shown below:

[Status check](https://user-images.githubusercontent.com/61512079/172871711-491799d0-b516-4dea-824d-f585028848a7.PNG "Apache Install Check")

Next port 80 was opened on the EC2 instance to allow inbound connections via the web.  

Output confirmed in file [Apache](https://user-images.githubusercontent.com/61512079/172872701-a73a3b60-7353-463b-b22e-17e17120eaed.PNG "Apache Web Confirm")

```http
http://<Public-IP-Address>:80  
```
  where Public-IP-Address is the public IP of the EC2 instance copied from this command: 
  ```bash
  curl -s http://169.254.169.254/latest/meta-data/public-ipv4
  ```
  
Next step is the installation of MySql database.
```bash
$ sudo apt install mysql-server
$ sudo mysql
```
Confirmation of MySql installation is logged in output shown below:
[MYSQL confirm](https://user-images.githubusercontent.com/61512079/172873312-d49ba2a1-89a9-4e56-a7a1-1cffae9f9900.PNG "MySQL Install Confirmation")

After the installation of the Mysql, next is the removal of some insecure default settings and lock down access to your database system.
```mysql
   ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
```
This is confirmed as shown in below:
[MySql-Sec](https://user-images.githubusercontent.com/61512079/172873796-e3e26ae4-02f5-4d62-9823-6002e76db3cc.PNG "MYSQL SECURITY CHECK")

```bash
$sudo mysql_secure_installation
```

Next is the installation of  PHP, the final component in the LAMP stack.
```bash
$sudo apt install php libapache2-mod-php php-mysql
```
Other dependencies were also installed with it: php-mysql & libapache2-mod-php

Once the installation is finished, logs output in
[PHP](https://user-images.githubusercontent.com/61512079/172874415-a7528f2d-b963-4d75-baa8-a520f4b9f654.PNG "PHP Installation") 

The following command was run to confirm  PHP version. Output in
[php VERSION](https://user-images.githubusercontent.com/61512079/172874768-e18bfbf0-a5f3-41fc-ab11-de248ce1b950.PNG "PHP Version COnfirm")

```bash
$php -v
```
FInally, a virtual host was create on the installed website first by creating a new subdirectory inside the default web root folder(/var/www).
```bash
$sudo mkdir /var/www/projectlamp
$sudo chown -R $USER:$USER /var/www/projectlamp
$sudo vi /etc/apache2/sites-available/projectlamp.conf
```
Edit the projectlamp.conf file with the script below:
```
  <VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
Run the command below to show the new file in the sites-available directory
```
$sudo ls /etc/apache2/sites-available
```

Enablinng the virtual host
```bash
$sudo a2ensite projectlamp
```
To disable the default apache website that was pre-installed
```
$sudo a2dissite 000-default
```
To make sure your configuration file doesn’t contain syntax errors, run:
```bash
$sudo apache2ctl configtest
```
Finally, reload Apache so these changes take effect:
```bash
$sudo systemctl reload apache2
```
To test the new website, open the link in a browser:
```http
  http://<Public-IP-Address>:80
```
  
In addition, PHP was enabled on the website with the commands below:
```
$sudo vim /etc/apache2/mods-enabled/dir.conf
```
Edit file with the script below:
```
<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
```
Reload apache for the configuration to take effect
```bash
$sudo systemctl reload apache2
```
FIle index.php was created  inside the custom web root folder and the script below was copied to it
```bash
$vim /var/www/projectlamp/index.php
```
```php
  <?php
phpinfo();
```
The PHP page was tested and output shown in [ ](https://user-images.githubusercontent.com/61512079/172875945-0811baf6-09e6-462f-a3b3-4f838496f507.PNG "PHP Page")

After confirming the working of the PHP information, it was removed to keep the sensitive information safe using the command below:
```bash
$sudo rm /var/www/projectlamp/index.php
```
