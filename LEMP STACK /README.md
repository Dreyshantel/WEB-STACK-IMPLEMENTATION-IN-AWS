# WEB STACK IMPLEMENTATION (LEMP STACK)- 101

## Overview

This project is a documentation guide to setting up LEMP (Linux, Nginx, MySQL, PHP) stack. The LEMP stack is an open source software often known for its flexibility, efficiency, and cost effectiveness. It is used to host a dynamic websites and web applications.

### Components

* **Linux**: The operating system on which the stack runs. Linux is chosen for its stability, security, and widespread support.
* **Nginx**: (pronounced as "engine-x") The web server software. It is known for its high performance, rich feature spirit, stability, simple configuration and low resources consumption
* **MySQL**: The relational database management system (RDBMS) used for storing and managing data. MySQL and MariaDB are popular choices due to their speed, reliability, and flexibility
* **PHP**: The programming language used to generate dynamic web content. PHP is the most commonly choice because of its simplicity and the large number of available framework and tools.



## Step 0- Preparing prerequisite

1. **Launch an ec2 instance**:

   - Sign in to AWS management console

   - Navigate to EC2 dashboard.

   - Click on "Launch instance" and choose Ubuntu Server 20.04 LTS as the operating system

![Screenshot (107)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/d5c6a3ad-88d6-433d-8783-6a1a3f020746)

![Screenshot (108)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/6b37154d-a5c7-45af-8e55-21917a380a53)




2. **Configure Instance Details:**

   - Choose instance type, network, subnet, and other required settings.

3.  **Add Storage**

   - Allocate storage space for according to your requirement

4.  **Add Tags:**

   - Optionally, add tags for better organization

5.  **Configure Security Group**

   - Create a new security group or use an existing one.
   - Allow inbound traffic on port 80 (HTTP, 433 (HTTPS), 22 (SHH) from your IP address

![Screenshot (141)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/f1735ab5-b8b0-437e-b7e6-f6bb5b8b6c45)


6. **Review and launch:**

   - Review the configuration and launch the instance.

![Screenshot (112)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/666b2093-ba7b-432e-9964-bed4d6992b18)


7. **Connect to the Instance**

   - use Git Bash to connect to the instance via SSH.

8.  **Grant permission for the private key and SSH into the instance**

   `chmod 400 my-ec2-key.pem`

   `ssh -i "my_ec2-key.pem" 16.16.223.129`

![Screenshot (114)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/d4f56243-8190-4a36-8a51-b9ab362eeb7b)


# 

## INSTALLING LEMP STACK

## Step 1- Installing the Nginx Web Browser

1. **Update Package Repository and install Nginx**

   `sudo apt update`

   `sudo apt install nginx`

![Screenshot (116)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/1773836a-dd5e-4a12-900e-16963ad99e49)




2. **Enable Nginx and verify if it is successfully running.**

   `sudo systemctl status nginx`

   

   if it is active and running, then nginx is successfully installed

![Screenshot (117)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/183fd98f-3e57-47f4-a24e-634636223ec4)




3. **The Server is successfully running and we can access it locally on in our Ubuntu shell**

   `sudo curl http://localhost:80`

   `sudo curl http://<public-IP-Address>:80`

![Screenshot (118)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/fc14b8f8-f070-49cc-a51d-6a2841e17201)


   

4. **Test our how Nginx server can respond to request from the internet.**

   `http://<public-IP-Address>:80`

   
![Screenshot (119)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/678c11d2-4289-4168-bb96-8a891369297b)






### Step 2- installing MySQL database

1. install MySQL database

   `sudo apt install mysql-server`

   ![Screenshot (120)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/091d2eef-12e0-417e-92f7-69e56cd06ee2)


2. **Logging into MySQL console**

   `sudo mysql`

   

3.  **Run a security script to set a password for root user, using mysql_native_password **

   `ALTERUSER'root'@'localhost' IDENTIFIED WITH mysql_native_password BY'Steghub';`

4. **Exit the MySQL shell**

   `exit`

5. **Start the interactive script by running**

   `sudo mysql_secure_installation`

![Screenshot (123)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/0395f667-d584-41c6-803c-f45ba1410a11)

![Screenshot (124)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/e2701ad4-acb7-472b-894e-0170626ecc87)


6. **Test if you're able to log in to the MySQL console**

   `sudo mysql -p`

![Screenshot (122)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/94033dc3-bafc-4200-ada1-2eeee66fadd9)


7. **Exit the MySQL console**

   `exit`



## Step 3 - Install PHP

1. **Install php:** While Apache embeds the PHP interpreter in each request, Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server. This allows for a better overall performance in most PHP-based websites.

   `sudo apt install php-fpm php-mysql`

   When prompted type y and press ENTER for confirmation

![Screenshot (125)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/5d2d7544-a122-41f0-98e3-1b332ee5cf77)
![Screenshot (126)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/8414fc1c-2e99-4fb0-87b2-d696d44653cf)




### Step 4 - Configuring Nginx to use PHP Processor

1. **Create a root directory for your domain and change owner permission**

   `sudo mkdir /var/www/projectLEMP`

2.  **Assign the directory ownership with $USER which reference the current system user**

   `sudo chown -R $USER$USER /var/www/projectLEMP`

![Screenshot (127)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/0d13b52c-2117-4bfa-a978-94da492da355)




3. **Create a new configuration file in Nginx sites-available directory**

   `sudo nano /etc/nginx/sites-available/projectLEMP`

   This will create a new blank file in the following bare-bones configuration. paste the following below:

   `server {
     listen 80;
     server_name projectLEMP www.projectLEMP;
     root /var/www/projectLEMP;

     index index.html index.htm index.php;

     location / {
       try_files $uri $uri/ =404;
     }

     location ~ \.php$ {
       include snippets/fastcgi-php.conf;
       fastcgi_pass unix:/var/run/php/php8.3-fpm.sock;
     }

     location ~ /\.ht {
       deny all;
     }

   }`

![Screenshot (128)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/94a95ac2-f472-45b3-bea9-92f7c4c33e07)


**Here's what each of these directives and location blocks do:**

* **listen** - Defines what port nginx listens on. In this case it will listen on port 80, the default port for HTTP.
* **root** - Defines the document root where the files served by this website are stored.
* **index** - Defines in which order Nginx will prioritize the index files for this website. It is a common practice to list index.html files with a higher precedence than index.php files to allow for quickly setting up a maintenance landing page for PHP applications. You can adjust these settings to better suit your application needs.
* **server_name** - Defines which domain name and/or IP addresses the server block should respond for. Point this directive to your domain name or public IP address.
* **location /** - The first location block includes the try_files directive, which checks for the existence of files or directories matching a URI request. If Nginx cannot find the appropriate result, it will return a 404 error.
* **location ~ .php$** - This location handles the actual PHP processing by pointing Nginx to the fastcgi-php.conf configuration file and the php7.4-fpm.sock file, which declares what socket is associated with php-fpm.
* **location ~ /.ht** - The last location block deals with .htaccess files, which Nginx does not process. By adding the deny all directive, if any .htaccess files happen to find their way into the document root, they will not be served to visitors. **4.** **Activate the configuration by linking to the config file from Nginx’s sites-enabled directory**

`sudo ln -s /etc/nginx/sites-available/projectLEMP /etc/nginx/sites-enabled/`

This will tell Nginx to use configuration next time it is reloaded



4. **Test configuration for syntax error**

   `sudo nginx -t`

5.  **Reload Nginx to apply the changes**

   `sudo systemctl reload nginx`

6.  **The new website is now active but the web root /var/www/projectLEMP is empty. Create an index.html file in this location so to test the virtual host work as expected.

   `sudo echo ‘Hello LEMP from hostname’ $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) ‘with public IP’ $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectLEMP/index.html`

![Screenshot (132)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/541c1ed2-74be-4f1e-a821-d65c1d60373d)




### Step 5 - Testing PHP with Nginx
1. **Create a PHP file in the document root. Open a new file called info.php within the document root.**

   `sudo nano /var/www/projectLEMP/info.php`

   paste the code below

   `<?php
   phpinfo();`

2.  **Access the page on the browser and attach /info.php**

`http://16.16.233.129`
![Screenshot (133)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/be62a3e2-3ef7-4fc3-b049-49244a06b5f7)


after checking the details about PHP is best to remove the file as it contain sensitive information

`sudo rm /var/www/projectLEMP/info.php`

### Step 6 - Retrieve Data from MySQL database with PHP

1. **Connect to MySQL database**

`sudo mysql`

2. **Create a new database**

   `CREATE DATABASE Mytodo_database;`

3.  **Create a user and grant the user full privilege with password**

   `CREATE USER 'example_user'@'%' IDENTIFIED WITH mysql_native_password BY '$Steghub24';`

   `GRANT ALL ON Mytodo_database.* TO 'todo_user'@'%'; `

   

4. **exit**

   `exit`

5.  **Check if the user has proper permission by logging **

   `mysql -u example_user -p`

   `SHOW DATABASES;`

6.  **Show database create a table called todo list**

   `CREATE TABLE todo_database.todo_list (
     item_id INT AUTO_INCREMENT,
     content VARCHAR(255),
     PRIMARY KEY(item_id)
   );`

7. **Insert few rows**

   `INSERT INTO todo_database.todo_list (content) VALUES ("My first important item");`

   `INSERT INTO todo_database.todo_list (content) VALUES ("My second important item");`

   `INSERT INTO todo_database.todo_list (content) VALUES ("My third important item");`
   ``INSERT INTO todo_database.todo_list (content) VALUES ("fourth important item");`
   `INSERT INTO todo_database.todo_list (content) VALUES ("My fifth important item");`
   `INSERT INTO todo_database.todo_list (content) VALUES ("and this one more thing");`

8.  **Check if that the data was successfully saved into the table**

   `SELECT * FROM example_database.todo_list;`

 ![Screenshot (136)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/bf6ddeee-1f3c-4030-bd9f-08e1997f4708)


9. **Exit** 

   `exit`

10. **Create a PHP script**

    `sudo nano /var/www/projectLEMP/todo_list.php`

11. **Copy this content into it**

    `<?php`
    `$user = "Ib";`
    `$password = "Admin123$";`
    `$database = "todo_database";`
    `$table = "todo_list";`

    `try {`
      `$db = new PDO("mysql:host=localhost;dbname=$database", $user, $password);`
      `echo "<h2>TODO</h2><ol>";`
      `foreach($db->query("SELECT content FROM $table") as $row) {`
        `echo "<li>" . $row['content'] . "</li>";`
      `}`
      `echo "</ol>";`
    `} catch (PDOException $e) {`
      `  print "Error!: " . $e->getMessage() . "<br/>```";
        `die();

    }

    ?>`
![Screenshot (139)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/9ea4dea1-cc2b-47dd-b487-14b6b0be5350)


12. **Save and close the file when you are done editing. Access the page by using IP address or domain name**

    `http://13.50.251.199/todo_list.php`

![Screenshot (144)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/eb50122b-8d4e-490c-ae22-adba5a60d6b9)


    

13. **Conclusion**

 By following the outlined steps, you can successfully set up a LEMP stack to create a powerful and scalable web hosting environment. This setup not only ensures optimal performance but also provides the flexibility needed for modern web applications.


