# Understanding Client-Server Architecture 
Client-Server refers to an architecture in which two or more computers are connected together over a network to send and receive requests between one another. In their communication, each machine has its own role: The machine sending requests is usually referred as "client" and the machine responding(serving) is called "Server".


A simple diagram of web Client-server architecture is presented below:

![Client-Server-Architecture-1](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/3323ce71-f557-47fe-be78-63d55ae76e4e)
- **Clients refer to the internet-connected devices used by typical web users, such as computers on Wi-Fi or phones on mobile networks, as well as the web-accessing software installed on these devices, typically web browsers like Firefox or Chrome.
Servers are computers that host webpages, websites, or applications. When a client device requests access to a webpage, a copy of that webpage is downloaded from the server and displayed in the user's web browser.**

The setup on the diagram above is a typical generic Web Stack architecture that we have already implemented in previous projects (LAMP, LEMP, MEAN, MERN), this architecture can be implemented with many other technologies - various Web and DB servers, from small Single-page applications SPA to large and complex portals.

### A quick client-server architecture in action
Open your Ubuntu or Windows terminal and run curl command:
```
curl -Iv steghub.com
```
![Screenshot (336)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/6a8778ee-14d9-40b1-958a-cb36d4690c05)

**Note:** If your Ubuntu does not have 'curl', you can install it by running sudo apt install curl In this example, your terminal or ubuntu will be the client, while steghub.com will be the server. The response from the remote server in above output. You can also see that the requests from the URL are being served by a computer with an IP address 160.153.133.153 on port 80

Another simple way to get a server's IP address is to use a simple diagnostic tool like 'ping', it will also show round-trip time - time for packets to go to and back from the server, this tool uses ICMP protocol.

### Implement a Client Server Architecture using MySQL Database Management System (DBMS).
To demonstrate a basic client-server using MySQL RDBMS, follow the below instructions
1. Create and configure two linux-based virtual server (EC2 instance in AWS)
```
Server A name - 'mysql server'
Server B name - 'mysql client'
```
 ### Setting up the 'mysql server'
1. **Launch an EC2 Instance:**
   - Sign in to the AWS Management Console.
   - Navigate to EC2 Dashboard.
   - Click on "Launch Instance" and choose Ubuntu Server 20.04 LTS as the operating system.
2. **Configure Instance Details:**
   - Choose instance type, network, subnet, and other settings as per your requirements.
3. **Add Storage:**
   - Allocate storage space according to your needs.
4. **Add Tags:**
   - Optionally, add tags for better organization.
5. **Configure Security Group:**
   - Create a new security group or use an existing one.
   - Allow inbound traffic on ports 80 (HTTP), 22 (SSH), 3306 (mysql) and 443 (HTTPS) from your IP address.
     ![Screenshot (337)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/57e4f2f7-9f26-430f-b17d-c4aafe494a52)
6. **Review and luanch instance**
   - Review the configuration and launch the instance.
     ![Screenshot (338)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/fb8ab8d8-b6cf-4b42-a87e-9751e34a4406)

### Setting up the 'mysql client
1. **Launch an EC2 Instance:**
   - Sign in to the AWS Management Console.
   - Navigate to EC2 Dashboard.
   - Click on "Launch Instance" and choose Ubuntu Server 20.04 LTS as the operating system.
2. **Configure Instance Details:**
   - Choose instance type, network, subnet, and other settings as per your requirements.
3. **Add Storage:**
   - Allocate storage space according to your needs.
4. **Add Tags:**
   - Optionally, add tags for better organization.
5. **Configure Security Group:**
   - Create a new security group or use an existing one.
   - Allow inbound traffic on ports 80 (HTTP), 22 (SSH), 3306 (mysql) and 443 (HTTPS) from your IP address.
     ![Screenshot (340)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/a56d904c-7d15-4c00-a53b-ceb4efa073ef)
6. **Review and launch**
   - Review the configuration and launch instance
     ![Screenshot (339)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/55377ce1-ce07-4392-adf0-4efcbdec966f)
**The two servers are up and running**

### Conect to mysql server using windows terminal
SSH into the instance
```
ssh -i "Steghub_keypair.pem" ubuntu@ec2-13-51-169-102
```
![Screenshot (342)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/6d071c2b-b312-4548-9569-1cb6fcc3ab55)

1. **Update Package Repository:**
```
sudo apt update
```
![Screenshot (343)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/b8f747d9-4882-4799-9cbb-ec2687fc9c84)
2.**Upgrade ubuntu:**
```
sudo apt upgrade
```
use the command below to install mysql
```
sudo apt install mysql-server -y
```
![Screenshot (344)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/26eef1da-2032-4c46-b54d-7280f1d1b7bd)
MySQL is an open-source relational database management system. Its name is a combination of "My", the name of the cofounder Michael Widenius's daughter, and "SQL", the abbreviation for Structured Query Language.
### conect to mysql client using windows terminal
**ssh to the instance**
```
ssh -i "Steghub_keypair.pem" ubuntu@ec2-13-60-30-240
```
![Screenshot (345)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/d797a342-cc75-421f-b260-437eec845bec)
1. **Update package repository**
   ```
   sudo apt update
   ```
![Screenshot (346)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/78345a2e-9896-4093-9563-d1e6912bb2b5)
2. **Upgrade Ubuntu**
```
sudo apt upgrade
```
3. **Install MySQL client software**
```
sudo apt install mysql-client -y
```
![Screenshot (347)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/9b65a1d1-760d-4f3f-ad89-9b2f207af751)
By default, the two EC2 virtual servers are located in the same local virtual network, so they can communicate to each other using local IP addresses.

**Edit inbound rule of mysql server to allow traffic into port 3306 , specifying only the mysql client ip address access**
![Screenshot (354)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/a7f765b3-544d-4ca7-8d04-68cd4751b3ac)

**configure MySQL server to allow connections from remote hosts.**
```
sudo vim /etc/mysql/mysql.conf.d/mysqld.cnf
```
Replace bind location = 127.0.0.1 with 0.0.0.0
![Screenshot (350)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/ddaef997-a5d3-41b4-96cc-e62c34e8ed0f)
Access MySQL shell
```
sudo mysql
```
create a user named 'client'with password
```
CREATE USER 'client'@'%' IDENTIFIED WITH mysql_native_password BY '$Timileyin';
```
create a databse called test_db
```
CREATE DATABASE test_db;
```
grant access to 'client' user
```
GRANT ALL ON test_db.* TO 'client'@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```
![Screenshot (351)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/aeb14377-8236-4f7a-a8ce-564c9b127ff0)
connect to mysql server using the command below

![Screenshot (356)](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/86636ad8-44a2-4223-89f5-22f96ebebfff)
**Check that you have successfully connected to a remote MySQL server and can perform SQL queries**
```
show databases;
```
![image](https://github.com/Dreyshantel/WEB-STACK-IMPLEMENTATION-IN-AWS/assets/109143806/2e8dead1-ef23-4e4f-a605-f5549f7b501e)

we have successfully completed this project - we have deployed a fully functional MySQL Client-Server set up

