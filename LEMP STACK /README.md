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

   ![Screenshot (107)](C:\Users\Shantel\Desktop\LEMP stack project\ec2 instance\Screenshot (107).png)

![Screenshot (108)](C:\Users\Shantel\Desktop\LEMP stack project\ec2 instance\Screenshot (108).png)



2. **Configure Instance Details:**

   - Choose instance type, network, subnet, and other required settings.

3.  **Add Storage**

   - Allocate storage space for according to your requirement

4.  **Add Tags:**

   - Optionally, add tags for better organization

5.  **Configure Security Group**

   - Create a new security group or use an existing one.
   - Allow inbound traffic on port 80 (HTTP, 433 (HTTPS), 22 (SHH) from your IP address

   ![Screenshot (141)](C:\Users\Shantel\Pictures\Screenshots\Screenshot (141).png)

6. **Review and launch:**

   - Review the configuration and launch the instance.

   ![Screenshot (112)](C:\Users\Shantel\Desktop\LEMP stack project\ec2 instance\Screenshot (112).png)

7. **Connect to the Instance**

   - use Git Bash to connect to the instance via SSH.

8.  **Grant permission for the private key and SSH into the instance**

   `chmod 400 my-ec2-key.pem`

   `ssh -i "my_ec2-key.pem" 16.16.223.129`

   ![Screenshot (114)](C:\Users\Shantel\Desktop\LEMP stack project\ec2 instance\Screenshot (114).png)

# 

## INSTALLING LEMP STACK

## Step 1- Installing the Nginx Web Browser

1. **Update Package Repository and install Nginx**

   `sudo apt update`

   `sudo apt install nginx`

   ![Screenshot (116)](C:\Users\Shantel\Desktop\LEMP stack project\install Nginx\Screenshot (116).png)



2. **Enable Nginx and verify if it is successfully running.**

   `sudo systemctl status nginx`

   

   if it is active and running, then nginx is successfully installed

   ![Screenshot (117)](C:\Users\Shantel\Desktop\LEMP stack project\install Nginx\Screenshot (117).png)



3. **The Server is successfully running and we can access it locally on in our Ubuntu shell**

   `sudo curl http://localhost:80`

   `sudo curl http://<public-IP-Address>:80`

   ![Screenshot (118)](C:\Users\Shantel\Desktop\LEMP stack project\install Nginx\Screenshot (118).png)

   

4. **Test our how Nginx server can respond to request from the internet.**

   `http://<public-IP-Address>:80`

   

![Screenshot (119)](C:\Users\Shantel\Desktop\LEMP stack project\install Nginx\Screenshot (119).png)





### Step 2- installing MySQL database

1. install MySQL database

   `sudo apt install mysql-server`

   

   ![Screenshot (120)](C:\Users\Shantel\Desktop\LEMP stack project\install RDBMS\Screenshot (120).png)

2. **Logging into MySQL console**

   `sudo mysql`

   

3.  **Run a security script to set a password for root user, using mysql_native_password **

   `ALTERUSER'root'@'localhost' IDENTIFIED WITH mysql_native_password BY'Steghub';`

4. **Exit the MySQL shell**

   `exit`

5. **Start the interactive script by running**

   `sudo mysql_secure_installation`

   ![Screenshot (123)](C:\Users\Shantel\Desktop\LEMP stack project\install RDBMS\Screenshot (123).png)

![Screenshot (124)](C:\Users\Shantel\Desktop\LEMP stack project\install RDBMS\Screenshot (124).png)



6. **Test if you're able to log in to the MySQL console**

   `sudo mysql -p`

   ![Screenshot (122)](C:\Users\Shantel\Desktop\LEMP stack project\install RDBMS\Screenshot (122).png)

7. **Exit the MySQL console**

   `exit`



## Step 3 - Install PHP

1. **Install php:** While Apache embeds the PHP interpreter in each request, Nginx requires an external program to handle PHP processing and act as a bridge between the PHP interpreter itself and the web server. This allows for a better overall performance in most PHP-based websites.

   `sudo apt install php-fpm php-mysql`

   When prompted type y and press ENTER for confirmation

   ![Screenshot (125)](C:\Users\Shantel\Desktop\LEMP stack project\install PHP\Screenshot (125).png)

   ![Screenshot (126)](C:\Users\Shantel\Desktop\LEMP stack project\install PHP\Screenshot (126).png)



### Step 4 - Configuring Nginx to use PHP Processor

1. **Create a root directory for your domain and change owner permission**

   `sudo mkdir /var/www/projectLEMP`

2.  **Assign the directory ownership with $USER which reference the current system user**

   `sudo chown -R $USER$USER /var/www/projectLEMP`

   ![Screenshot (127)](C:\Users\Shantel\Desktop\LEMP stack project\configure nginx\Screenshot (127).png)



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

   ![Screenshot (128)](C:\Users\Shantel\Desktop\LEMP stack project\configure nginx\Screenshot (128).png)

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

   ![Screenshot (132)](C:\Users\Shantel\Desktop\LEMP stack project\configure nginx\Screenshot (132).png)



### Step 5 - Testing PHP with Nginx

1. **Create a PHP file in the document root. Open a new file called info.php within the document root.**

   `sudo nano /var/www/projectLEMP/info.php`

   paste the code below

   `<?php
   phpinfo();`

2.  **Access the page on the browser and attach /info.php**

`http://16.16.233.129`

![Screenshot (133)](C:\Users\Shantel\Desktop\LEMP stack project\Testing PHP with Nginx\Screenshot (133).png)

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

   ![Screenshot (136)](C:\Users\Shantel\Pictures\Screenshots\Screenshot (136).png)

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

![image-20240602044931055](C:\Users\Shantel\AppData\Roaming\Typora\typora-user-images\image-20240602044931055.png)

12. **Save and close the file when you are done editing. Access the page by using IP address or domain name**

    `http://13.50.251.199/todo_list.php`

    ![Screenshot (144)](C:\Users\Shantel\Pictures\Screenshots\Screenshot (144).png)

    

13. **Conclusion**

 By following the outlined steps, you can successfully set up a LEMP stack to create a powerful and scalable web hosting environment. This setup not only ensures optimal performance but also provides the flexibility needed for modern web applications.


