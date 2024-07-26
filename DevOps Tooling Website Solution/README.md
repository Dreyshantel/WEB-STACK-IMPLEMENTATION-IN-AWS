# DevOps Tooling Website Solution
### Overview
This project introduce a set of DevOps tools that will help our team in day to day activities in managing, developing, testing, deploying and monitoring different projects.

### Setup and Technologies
In this project, a tooling website solution which makes access to DevOps tools within the corporate infrastructure easily accessible will be implemented. These consist of following components:
1. **Infrastructure:** AWS
2. **Webserver Linux:** Red Hat Enterprise Linux 8
3. **Storage Server:** Red Hat Enterprise Linux 8 + NFS Server
4. **Database server:** Ubuntu 24.04 + MySQL
5. **Programming language:** PHP
6. **Code Repository:** GitHub


### Step 1- Prepare NFS Server
1. Spin up a new EC2 instance with RHEL Linux 8 Operating system
![Screenshot (510)](https://github.com/user-attachments/assets/20ba2a24-cee9-488e-a6a8-6ff075299f03)

2. Configure the inbound rules to allow traffic from SSH, HTTP, and HTTPS traffic from the internet
![Screenshot (511)](https://github.com/user-attachments/assets/57547066-17f9-4f47-908f-7c7dee7be571)

3. Configure storage and add 3 volumes to the EC2 instance
![Screenshot (512)](https://github.com/user-attachments/assets/bea72110-2013-4125-94f6-5e561482f8bb)

4. Use "lsblk" command to inspect what storage block is attached to the server
![Screenshot (513)](https://github.com/user-attachments/assets/5e1290e6-5d53-4483-82e7-ffce09611be8)

5. Use gdisk to create a single partition on each of the 3 disk
![Screenshot (514)](https://github.com/user-attachments/assets/e9acb228-dbbc-401a-a158-2b21a59264ae)

use "lsblk" command to confirm creation of partition of the 3 disk
![image](https://github.com/user-attachments/assets/313c2d50-92da-4a15-b41a-7f7fc9cf0d1d)

6. Update package repository and install lvm2 on the server.
![Screenshot (516)](https://github.com/user-attachments/assets/026cd05e-d00c-43f8-aaa5-183c3a8a9946)

Use `lvmdiskscan` to check available partitions
![image](https://github.com/user-attachments/assets/39fcbf2a-740f-4541-accd-a4e137d0c496)

7. Use the lvcreate utility to create logical volumes
```
sudo lvcreate -n lv-apps -L NFS-vg
sudo lvcreate -n lv-logs -L NFS-vg
sudo lvcreate -n lv-opt -L NFS-vg
```
![image](https://github.com/user-attachments/assets/bcc6fd6a-901e-4dac-a3f0-c17caa1a3492)

8. Use mkfs.xfs to format the logical volumes with xfs filesystem
```
sudo mkfs -t xfs dev/NFS-vg/lv-apps
sudo mkfs -t xfs /dev/NFS-vg/lv-logs
sudo mkfs -t xfs /dev/NFS-vg/lv-opt
```
![Screenshot (519)](https://github.com/user-attachments/assets/4b81749b-2136-478a-abf3-57529cd88b89)


9. Create mount points on /mnt directory for the logical volumes as follows: Mount lv-apps on /mnt/apps- To be used by webservers. Mount lv-logs on /mnt/logs - To be used by webserver logs. Mount lv-opt on /mnt/opt- To be used by Jenkins server in project 8
```
sudo mount /dev/NFS-vg/lv-apps /mnt/apps
sudo mount /dev/NFS-vg/lv-logs /mnt/logs
sudo mount /dev/NFS-vg/lv-opt  /mnt/opt
```

10. Install NFS Server, configure it to start on reboot and make sure it us up and running
```
sudo yum -y update
sudo yum install nfs-utils -y
sudo systemctl start nfs-server.service
sudo systemctl enable nfs-server.service
sudo systemctl status nfs-server.service
```
![Screenshot (520)](https://github.com/user-attachments/assets/74ed350f-a149-4ed5-97a6-55935a826067)

11. Export the mounts for webservers' subnet cidr to connect as clients. For simplicity, install all three web servers inside the same subnet, but in production set up you would probably want to seperate each tier inside its own subnet for higher level of security. To check your subnet cidr - Open your EC2 details in AWS web console and locate 'networking' tab and open a subnet link

12. Make sure to set up a permission that will allow our web servers to read, write and execute files on NFS
```
sudo chown -R nobody: /mnt/apps
sudo chown -R nobody: /mnt/logs
sudo chown -R nobody: /mnt/opt

sudo chmod -R 777 /mnt/apps
sudo chmod -R 777 /mnt/logs
sudo chmod -R 777 /mnt/opt
    
sudo systemctl restart nfs-server.service
```

13. Configure access to NFS for clients within the same subnet
```
sudo vi /etc/exports

sudo vi /etc/exports

/mnt/apps 172.31.16.0/20(rw,sync,no_all_squash,no_root_squash)
/mnt/logs 172.31.16.0/20(rw,sync,no_all_squash,no_root_squash)
/mnt/opt 172.31.16.0/20(rw,sync,no_all_squash,no_root_squash)

sudo exportfs -arv
```
![image](https://github.com/user-attachments/assets/8fc15a62-4e3b-49ce-be8d-e6b1803f06f2)
![image](https://github.com/user-attachments/assets/40006c14-8b88-4b22-a85e-353d1aabaabd)

14. Check which port is used by NFS and open it using security groups (add new inbound groups)
    ![image](https://github.com/user-attachments/assets/d90d663e-9b24-4755-b108-7796a4b317d5)

 **Important note:** In order for NFS server to be accessible from your client, you must also open following ports: TCP 111, UDP 111, UDP 2049
 ![Screenshot (525)](https://github.com/user-attachments/assets/f93313ce-550d-4126-922f-bdccff5b8b5e)


 ### Step 2 - Configure the database server
 1. Set up a database server with the necessary details needed
![Screenshot (526)](https://github.com/user-attachments/assets/bb027085-04fe-467b-98ee-824003cd2c92)

 2. update package repository and install mysql server
```
sudo yum -y update
sudo yum install mysql-server
```
![Screenshot (527)](https://github.com/user-attachments/assets/bee5fa5f-4897-4094-acbf-a7c102b008b9)

**Run mysql secure installation**
![Screenshot (528)](https://github.com/user-attachments/assets/efab5e3d-11ae-4db9-b5f5-3af3d111c929)

Create a database and name it tooling Create a database user and name it webaccess Grant permission to webaccess user on tooling database to do anything only from the webservers subnet cidr
```
sudo mysql

CREATE DATABASES tooling;
CREATE USER 'webaccess'@'172.31.16.0/20' IDENTIFIED WITH mysql_native_password BY '$mypass';
GRANT ALL PRIVILEGES ON tooling.* 'webaccess'@'172.31.16.0/20' WITH GRANT OPTION;
FLUSH PRIVILEGES;

use tooling
select host, user from mysql.user;
exit
```
![Screenshot (529)](https://github.com/user-attachments/assets/745aaf4e-9240-4a5f-9d22-29a824f46b36)


Set Bind Address and restart MySQL
```
sudo vi /etc/my.cnf

sudo systemctl restart mysqld
sudo systemctl status mysqld
```
![Screenshot (530)](https://github.com/user-attachments/assets/1a9f5446-5f7e-44de-be0d-a14cb2e8d361)
![Screenshot (531)](https://github.com/user-attachments/assets/82a78428-308f-481d-93f9-f91bbf60468b)


### Step 3 - Prepare the web servers
We need to make sure that our Web Servers can serve the same content from shared storage solutions, in our case - NFS Server and MySQL database. During the next steps the following will be done:

1. Configure NFS client (this step must be done on all three servers)
2. Deploy a Tooling application to our Web Servers into a shared NFS folder
3. Configure the Web Servers to work with a single MySQL database.
4. Launch a new EC2 instance with RHEL 8 Operating System
![Screenshot (532)](https://github.com/user-attachments/assets/bc53e24f-9a70-4c31-a715-89c06819d617)

5. Install NFS client
```
sudo yum install nfs-utils nfs4-acl-tools -y
```
![Screenshot (533)](https://github.com/user-attachments/assets/70af4e51-51ea-46eb-8c27-86782c1580f9)

6. Mount /var/www/ and target the NFS server's export for apps
```
sudo mkdir /var/www
sudo mount -t nfs -o rw,nosuid 172.31.24.35:/mnt/apps /var/www
```

7. Verify that NFS was mounted successfully by running df -h. Make sure that the changes will persist on web server after reboot
![image](https://github.com/user-attachments/assets/8059fbf8-de71-4bf1-87f8-b7b0632b0228)
![image](https://github.com/user-attachments/assets/e1b9eaae-3ac4-4f7d-a977-9fec909dd556)

8. Install Remi's repository, Apache and PHP
```
sudo yum install httpd -y
```
![Screenshot (536)](https://github.com/user-attachments/assets/95bfa40d-5ce3-4f5b-a57f-dbd19a43e99b)




```
sudo dnf install https://fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-8.rpm
```

```
sudo dnf module reset php
sudo dnf module enable php:remi-7.4
sudo dnf install php php-opcache php-gd php-curl php-mysqlnd
```

```
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
sudo systemctl status php-fpm

sudo setsebool -P httpd_execmem 1  # Allows the Apache HTTP server (httpd) to execute memory that it can also write to. This is often needed for certain types of dynamic content and applications that may need to generate and execute code at runtime.
sudo setsebool -P httpd_can_network_connect=1   # Allows the Apache HTTP server to make network connections to other servers.
sudo setsebool -P httpd_can_network_connect_db=1  # allows the Apache HTTP server to connect to remote database servers.
```

### Web server 2
1. Launch a new EC2 instance with RHEL 8 Operating System
![Screenshot (537)](https://github.com/user-attachments/assets/a1ef30d0-8358-49f9-b4bf-b6b6a88a1b06)

2. Install NFS client
```
sudo yum install nfs-utils nfs4-acl-tools -y
```
![Screenshot (538)](https://github.com/user-attachments/assets/3456fb1b-d42b-4453-9186-b5a36f6ad646)

3. Mount /var/www/ and target the NFS server's export for apps
```
sudo mkdir /var/www
sudo mount -t nfs -o rw,nosuid 172.31.28.107:/mnt/apps /var/www
```


4. Verify that NFS was mounted successfully by running df -h. Make sure that the changes will persist on Web Server after reboot:
![image](https://github.com/user-attachments/assets/3bb3dd3b-e431-4f11-a71c-234de1d33bb7)

```
sudo vi /etc/fstab
```

Add the following
```
172.31.24.35:/mnt/apps /var/www nfs defaults 0 0
```
![image](https://github.com/user-attachments/assets/098a7e74-b593-4efb-9a97-537e98629866)

5. Install Remi's repository, Apache and PHP
```
sudo yum install httpd -y
```

```
sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
```
![Screenshot (543)](https://github.com/user-attachments/assets/603e53c4-4c18-469c-b03d-c505223a6b51)


```
sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-9.rpm
```

```
sudo dnf module enable php:remi-8.2
sudo dnf install php php-opcache php-gd php-curl php-mysqlnd
```

```
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
sudo systemctl status php-fpm

sudo setsebool -P httpd_execmem 1  # Allows the Apache HTTP server (httpd) to execute memory that it can also write to. This is often needed for certain types of dynamic content and applications that may need to generate and execute code at runtime.
sudo setsebool -P httpd_can_network_connect=1   # Allows the Apache HTTP server to make network connections to other servers.
sudo setsebool -P httpd_can_network_connect_db=1  # allows the Apache HTTP server to connect to remote database servers.
```


