# WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS

## WHAT IS A LAMP STACK?
A lamp stack is an open source software stack commonly used in creating a web page. The lamp stack is known for it's flexibility, stability and cost effectiveness, making it the best choice in creating web pages. The acronym LAMP stands for:
1. Linux: An operating system used to host the web server
2. Apache: A web server softer which server the web pages to users browser
3. MySQL: Relational database management system (RDBMS) used to store and manage data
4. PHP: a server-side scripting language used to create a dynamic web pages  


## STEP 0 - PREPARING PREREQUISITE
### CREATE AN EC2 INSTANCE
1. Created an ec2 instance with a t2 micro free tier virtual server type, which is located in us-east-1 (N. Virginia) . The ec2 instance is being run on an ubuntu server 24.0 LTS (HVM) SSD volume type
  

![Screenshot (44)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/assets/109143806/b99a5a02-11ad-4693-bf9c-bfeb5f3ce34f)
![Screenshot (45)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/assets/109143806/cd25fc49-ba5e-4ec3-b438-cf949964fd7d)



2. Created SSH key-pair named (steghub-keypair) to access the remote server from port 22

   
![Screenshot (46)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/assets/109143806/f9ef9586-87bc-406e-b146-1f549fee6ce1)


3. Modified the network security group to allow HTTP port 80, and HTTPS port 443 traffic from the internet


![Screenshot (47)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/assets/109143806/42932d19-bae4-4384-b16d-2be9ea12b3a5)


4. The ec2 instance has been successfully launched and running!

![Screenshot (48)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/assets/109143806/a7cfe836-718a-48e9-8034-92acaf58fced)


5. The SSH private key-pair downloaded was located, and the permission command 'chmod 400 "steghub_keypair.pem' was runned to ensure the private key is not publicly viewable
6. the command 'ssh -i "steghub_keypair.pem" ubuntu@ec2-3-90-190-154.compute-1.amazonaws.com' would be used to SSH from a local machine terminal (mobaxterm terminal)  to my remote ec2 instance server

![image](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/assets/109143806/e5c5cf7b-1da6-4759-b614-125e9a3fb330)  


# STEP 1 - INSTALLING APACHE2 HTTP WEB-SERVER AND UPDATING FIREWALL
1: Update system packages using 'sudo apt update' command.

![Screenshot (50)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/assets/109143806/16d5ac92-76ac-479c-8a7c-2a0e06195af5)


2. Installed apache2 http web-server using 'sudo apt install apache2' command.

![Screenshot (51)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/assets/109143806/22b030c8-f81f-4039-99a4-a70b0864ae6c)
![Screenshot (52)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/assets/109143806/d86ca1b0-c9ac-4bc0-a2bc-fe8739373ce0)


3. Checked if apache2 web-server is actively running using 'sudo systemctl status apache2' command, and after checking apache2 server status, we can access our our web server on the local server with the "sudo curl http://54.227.205.81:80" command.

![Screenshot (53)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/assets/109143806/e34169e4-4b37-4314-94ac-a9a3cc9e7f43)
![Screenshot (54)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/assets/109143806/76ee2a95-d118-47fb-8459-e459b34ece82)


4. Successfully launched apache2 web-server on the internet.

![Screenshot (55)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/assets/109143806/57b29c03-8b44-4260-94e7-b1c7a4462201)    




# STEP 2 - INSTALLED RELATIONAL DATABASE MANAGEMENT SYSTEM (RDBMS) MySQL
1. Installing MySQL using the 'sudo apt install msql_server' command.

![Screenshot (56)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/assets/109143806/dd911781-a05d-4a4c-9441-d367edb5dbcb)
![Screenshot (57)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/assets/109143806/6992b8f6-1095-49f6-8fad-820cccd820af)


2. Access MySQL using 'sudo mysql -p' command.


![Screenshot (58)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/assets/109143806/4c0483c7-947c-423d-92b6-b5f04e5a41a5)

3. Configure MySQL security/installation settings with 'sudo mysql_secure_installation' command 

![Screenshot (60)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/assets/109143806/ce2daa7b-2227-4b92-bd37-5f1db9d0cebd)
![Screenshot (61)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/assets/109143806/b265d3e3-ed80-43d6-81c7-b3c92732387c)


# STEP 3 - INSTALLING PHP
1. installing PHP and it's component/module with 'sudo apt install php libapache2-mod-php php-mysql', and check php version with 'sudo php -v' command.

![Screenshot (64)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/assets/109143806/0f5a91b0-1b47-4eb9-8ad7-cbd6a3213bb5)
![Screenshot (65)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/assets/109143806/a2d2021f-3ce5-4f6b-ba88-878069795884)
![Screenshot (66)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/assets/109143806/9db2dff4-3bc0-4742-a88a-7f49876ef7d2)


# STEP 4 - CREATING A VIRTUAL HOST FOR THE WEBSITE USING APACHE
Below are steps on how to create a virtual host for our website
1. create a directory for projectlamp using mkdir command
2. assign ownership of the directory to current system user using the 'chown' command
3. create a new configuration file in APACHE's sites-available directory
4. enable new virtual host with 'sudo a2ensite projectlamp' command
5. disable apache default website using 'sudo a2dissite 000-defualt' command
6. reload apache and create an index.html file in /var/www/projectlamp web root

![Screenshot (72)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/assets/109143806/ef13a226-8175-4015-951a-ca494d2777bd)
![Screenshot (67)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/assets/109143806/16464ca9-a7de-44d9-a730-1077a8a88ab5)


7. Enable instance metadata option and and set IMDSv2 to optional

![Screenshot (69)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/assets/109143806/bd52db2f-dfde-46f5-af05-1b217ee50150)


8. Finally our website is up and running on the internet

![Screenshot (70)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/assets/109143806/02419a47-827e-4225-9a58-34ab812ce34f)


# STEP 5 - ENABLINg PHP ON THE WEBSITE
1. with the default directoryindex on apache. A file name index.html will take precedence over an index.php file. enable dir/conf and change the order in which the index.php file is  listed in the directory directive




























