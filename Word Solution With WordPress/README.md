# WORD SOLUTION WITH WORDPRESS
## Overview
This project is a documentation guide on how to prepare storage infrastructure on two linux servers and implement a basic wordpress solution using WordPress. WordPress is a  free and open source content management system written in PHP and paired with MySQL and MariaDB as its backend relational database management system (RDBMS)

### Project Tasks
1. Configure storage subsystem for web and Database servers based on Linux OS
2. Install WordPress and connect it to a remote MySQL database server

## Three-tier Architecture
Three-tier architecture is a client-server architecture pattern that divides applications into three logical and physical computing tiers, or layers. Each tier is responsible for a specific aspect of the application, ensuring separation of concerns and improving scalability, maintainability, and manageability. Generally, web, or mobile solutions are implemented based on what is called the Three-tier Architecture.

**Three-tier Architecture** is a client-server software architecture pattern that comprise of 3 seperate layers.
![Three-tier application](https://github.com/user-attachments/assets/20d7c28f-2de9-4292-98ef-d97ca1345b99)

1. **Presentation tier**(Client tier): The presentation tier is the topmost layer of the application. It is responsible for interacting with the end users, presenting data, and capturing user input. This tier is often implemented using web browsers, mobile apps, or desktop applications. It provides a graphical interface for users to interact with the application. This includes rendering HTML, CSS, and JavaScript for web applications or native components for mobile and desktop applications. It captures user input, such as submission information, button clicks and other interactions, and receives data from the application tier and displays it to the users in a readable format 
2. **Business Layer**(Logic tier): The application tier, also known as the logic tier or middle tier, is responsible for the core functionality and business logic of the application. It acts as an intermediary between the presentation tier and the data tier, processing user inputs, making logical decisions, and coordinating data flow
3. **Data Access or Management tier**(Storage tier): The data tier is the bottom layer of the application architecture. It is responsible for storing, retrieving, and managing data. This tier ensures data persistence and provides mechanisms for data querying and manipulation.

### The 3-Tier Setup
1. A laptop or PC to serve as a client
2. An EC2 Linux Server as a web server (This is where you will install WordPress)
3. An EC2 Linux server as a database (DB) server


### SETTING UP THE EC2 INSTANCE
1. **Launch an EC2 instance**
   - sign in to the AWS management console
   - Navigate to EC2 instance dashboard
   - Click on launch "Launch instance" and choose Red Hat Enterprise Linux 9 (HVM)
     ![Screenshot (446)](https://github.com/user-attachments/assets/f1ef5609-9122-4113-8d7e-58c706df32f8)

2. **Configure instance details**
   - Choose instance type, network subnet and other required settings

3. **Add Storage**
   - Allocate storage space according to your requirement

4. **Add Tags**
   - Optionally, add tag for better organinzation

5. **Configure security group**
   - Create a new security group or use an existing one.
   - Allow inbound traffic on port 80 (HTTP), 433 (HTTPS), and port 22 (SSH) from your IP address

6. **Review and launch**
   - Review the configuration and launch the instance 

7. **Connect to the instance**
   - connect to your server through your preferred terminal via SSH
     ![image](https://github.com/user-attachments/assets/24176bbb-10f5-463e-8d0e-0c89670856f6)

   
### Step 1- Prepare a Web server
**Create 3 Vin the same AZ as your web server EC2, each of 10 GiB** 
![Screenshot (453)](https://github.com/user-attachments/assets/31698e35-76ba-46fe-a3d4-cf61e57c017f)


**The Volumes has been created**
![Screenshot (456)](https://github.com/user-attachments/assets/ca038b74-881d-417d-bb62-3720bbb1fb77)


**Attach all three volumes one by one to the web server EC2 instance**
   ![Screenshot (458)](https://github.com/user-attachments/assets/9165127d-b495-4861-94c3-35dce90d34b1)


**Open the terminal to start configuration**
  - use lsblk command to inpsect what block devices are attached to the server
```
    lsblk
```

![image](https://github.com/user-attachments/assets/ae8bfdc0-c56f-406a-9a5b-1baaa2eba1c7)

**Use df -h command to see all mounts and free space on your server**
```
df -h
```


**Use gdisk utility to create a single partition on each of the 3 disks**
```
sudo gdisk /dev/
```
![Screenshot (461)](https://github.com/user-attachments/assets/79b39cb3-3622-43b8-b739-a388681dc114)


**Use lsblk utility to view the newly configured partiton on each of the 3 disks**
```
lsblk
```
![image](https://github.com/user-attachments/assets/c7240e31-8a80-4fc8-8970-66f20a831fa1)


**Install lvm2 package using sudo yum install lvm2. Run sudo lvmdiskscan command to check for available partition**
   ```
   sudo yum install lvm2
   sudo lvmdiskscan
   ```
![Screenshot (463)](https://github.com/user-attachments/assets/2b372844-a411-4da3-8e11-7d1f79459ef5)
![image](https://github.com/user-attachments/assets/84cdeac6-f81c-4006-b489-e6b55ab38054)

**Note:** In ubuntu we use apt command to install packages, in RedHat/CentOS a different package manager is used, so we shall use yum command instead


**Use pvcreate utility to mark each of 3 disks as physical volumes(PVs) to be used by LVM**
   ```
    sudo pvcreate /dev/nvme1n1p1
    sudo pvcreate /dev/nvme2n1p1
    sudo pvcreate /dev/nvme3n1p1
   ```
![image](https://github.com/user-attachments/assets/eea43d8e-137d-4815-8b92-ae828ae0a702)


**Verify that your physical volume has been created successfully by running sudo pvs**
```
sudo pvs
```
![image](https://github.com/user-attachments/assets/76fefb4f-8e1e-4ffa-b362-2f9db63303f6)


**Use vgcreate utility to add all 3 PVs to a volume group (VG). Name the VG webdata-vg**
```
sudo vgcreate webdata-vg /dev/nvme1n1p1 /dev/nvme2n1p1 /dev/nvme3n1p1
```
**verify that your VG has been created successfully by running sudo vgs**
```
sudo vgs
```

**Use lvcreate utility to create 2 logical volumes. apps-Iv (Use half of the PV size), and logs-iv Use the remaining space of the PV size. Note: apps-Iv will be used to store data for the website while, logs-Iv will be used to store data for logs.**
```
sudo lvcreate -n apps-lv -L 14G webdata-vg
sudo lvcreate -n logs-lv -L 14G webdata-vg
```
![image](https://github.com/user-attachments/assets/dade8aee-eb1d-49de-b936-deb1c77fd487)

**Verify that your logical Volume has been created successfully by running sudo lvs**
```
sudo lvs
```
![image](https://github.com/user-attachments/assets/4e3c58b9-f4a1-4ad1-b2f1-6c4e6dc77834)

**Verify the entire set up**
```
sudo vgdisplay -v #view complete setup -VG, PV, and LV
sudo lsblk
```
![Screenshot (470)](https://github.com/user-attachments/assets/069ed7f5-7807-48bc-b965-23d13f372a97)
![Screenshot (472)](https://github.com/user-attachments/assets/cf7ad70e-47b5-489b-a13d-0020803ca234)

**Use mkfs.ext4 to format the logical volumes with ext4 filesystem**
![Screenshot (473)](https://github.com/user-attachments/assets/a3f74e6b-5e90-4adc-94bb-33e9b7a4f2ed)

Create **/var/www/html** directory to store website files sudo mkdir -p /var/www/html
Create **/home/recovery/logs** to store backup of log data sudo mkdir -p /home/recovery/logs
Mount **/var/www/html** on **apps-lv** logical volume
```
sudo mount /dev/webdata-vg/apps-lv /var/www/html/
```

**Use rsync utility to backup all the files in the log directory /var/log into /home/recovery/logs (This is required before mounting the file system)**
```
sudo rsync -av /var/log /home/recovery/logs/
```
![Screenshot (474)](https://github.com/user-attachments/assets/fbf6a459-a49a-46b5-b2c6-1c488beb3a4e)

** mount /var/log on logs-iv logical volume. (Note that all the existing data on /var/log will be deleted)
```
sudo mount /dev/webdata-vg/logs-lv /var/log
```
**Restore log files back into /var/log directory**
```
sudo rsync -av /home/recovery/logs/ /var/log
```
**Update /etc/fstab file so that the mount configuration will persist after restart of the server**
The UUID of the device will be used to update the /etc/fstab file;
```
sudo blkid
```
![image](https://github.com/user-attachments/assets/d5349d6b-dcc1-4987-a4d9-9028a41fb964)


``` sudo vi /etc/fstab ``` Update /etc/fstab in this format using your own UUID and remember to remove the leading and ending quotes.
![image](https://github.com/user-attachments/assets/08c663d1-7a62-41f1-b165-853ddcadee23)

**Test the configuration and reload the daemon**
```
sudo mount -a
sudo systemctl daemon-reload
```
**Verify your setup by running df -h, output must look like this:**
![image](https://github.com/user-attachments/assets/a24b2d80-174b-4775-a3f8-4a8be8d82428)

### Step 2 -  Prepere The Database
Launch a second RedHat EC2 instance that will have a role - 'DB Server' Repeat the same steps as for thr web Server, but instead of apps-lv create db-lv and mount it to /db directory instead of /var/www/html/.

 
 1. **Launch an EC2 instance**
   - sign in to the AWS management console
   - Navigate to EC2 instance dashboard
   - Click on launch "Launch instance" and choose Red Hat Enterprise Linux 9 (HVM)
     ![Screenshot (446)](https://github.com/user-attachments/assets/f1ef5609-9122-4113-8d7e-58c706df32f8)

2. **Configure instance details**
   - Choose instance type, network subnet and other required settings

3. **Add Storage**
   - Allocate storage space according to your requirement

4. **Add Tags**
   - Optionally, add tag for better organinzation

5. **Configure security group**
   - Create a new security group or use an existing one.
   - Allow inbound traffic on port 80 (HTTP), 433 (HTTPS), and port 22 (SSH) from your IP address

6. **Review and launch**
   - Review the configuration and launch the instance 

7. **Connect to the instance**
   - connect to your server through your preferred terminal via SSH

2. **Configure instance details**
   - Choose instance type, network subnet and other required settings

3. **Add Storage**
   - Allocate storage space according to your requirement

4. **Add Tags**
   - Optionally, add tag for better organinzation

5. **Configure security group**
   - Create a new security group or use an existing one.
   - Allow inbound traffic on port 80 (HTTP), 433 (HTTPS), and port 22 (SSH) from your IP address

6. **Review and launch**
   - Review the configuration and launch the instance 

7. **Connect to the instance**
   - connect to your server through your preferred terminal via SSH
![Screenshot (480)](https://github.com/user-attachments/assets/8e8c3582-e71d-4b35-b892-69653397ecb3)

Repeat the same steps as for the Web Server, but instead of appslv create db-lv and mount it to /db directory instead of /var/www/html/. ". Create 3 volumes in the same AZ as your Web Server EC2, each of 10 GiB.
![image](https://github.com/user-attachments/assets/6388fa98-6968-43f4-ae70-5b510ebcc582)

**Use lsblk command to inspect what block devices are attached to the server.**
```
lsblk
```
![image](https://github.com/user-attachments/assets/cb194962-dcd1-4e82-93ba-0b5010b38d76)

**Use df -h to see all mounts and free space on the server.**
```
df -h
```
![image](https://github.com/user-attachments/assets/61794e08-40b4-4127-90d9-cf4e4b0443be)

**Use gdisk utility to create a single partition on each of the 3 disks**
```
sudo gdisk /dev/nvme1n1
sudo gdisk /dev/nvme2n1
sudo gdisk /dev/nvme3n1
```
![Screenshot (484)](https://github.com/user-attachments/assets/2c70f928-10b7-4f75-8d48-ee020edfde91)

**Use lsblk utility to view the newly configured partition on each of the 3 disks.**
```
lsblk
```
![image](https://github.com/user-attachments/assets/0c752d93-e2c3-4f37-98ea-653057e660ef)

**Install lvm2 package using:**
```
sudo yum install lvm2
```
![Screenshot (486)](https://github.com/user-attachments/assets/9077ae91-cca1-4eb5-a29b-230b0f6889a0)

**Run sudo ```lvmdiskscan``` command to check for available partitions**
![image](https://github.com/user-attachments/assets/807f2424-3463-4bc9-b353-16b22b6974aa)

**Use pvcreate utility to mark each of 3 disks as physical volumes (PVs) to be used by LVM**
```
sudo pvcreate /dev/nvme1n1p1
sudo pvcreate /dev/nvme2n1p1
sudo pvcreate /dev/nvme3n1p1
```
![image](https://github.com/user-attachments/assets/422a07aa-e401-4a3e-ad38-9265120f281a)


**Verify that your Physical volume has been created successfully by running sudo pvs sudo pvs**
![image](https://github.com/user-attachments/assets/47b6dc89-5ef9-4708-9707-d1071ac1ce65)

**Use vgcreate utility to add all 3 PVs to a volume group (VG). Name the VG webdata-vg**
```
sudo vgcreate database-vg /dev/nvme1n1p1 /dev/nvme2n1p1 /dev/nvme3n1p1
```

**Verify that your VG has been created successfully by running ```sudo vgs``**
![image](https://github.com/user-attachments/assets/bbbcbea4-3d40-471f-b2fb-d762bab122ae)


**Use lvcreate utility to create a logical volume, db-lv (Use 20G of the PV size since it is the only LV to be created). Verify that the logical volumes have been created successfully**
```
sudo lvcreate -n db-lv -L 20G database-vg
sudo lvs
```
![image](https://github.com/user-attachments/assets/a9a15273-2683-4905-8c21-ac5d04ecaf15)

**Use mkfs.ext4 to format the logical volumes with ext4 filesystem and monut /db on db-lv**
```
sudo mkfs.ext4 /dev/database-vg/db-lv
sudo mount /dev/database-vg/db-lv /db
```

**Update /etc/fstab file so that the mount configuration will persist after restart of the server Get the UUID of the device**
```
sudo blkid
```
**Update the /etc/fstab file with the format shown inside the file using the UUID. Remember to remove the leading and ending quotes**
```
sudo vi /etc/fstab
```
![image](https://github.com/user-attachments/assets/e28a070a-81b2-4e20-b418-53eea4e6aa1c)

**Test the configuration and reload daemon. Verify the setup**
```
sudo mount -a   # Test the configuration

sudo systemctl daemon-reload

df -h   # Verifies the setup
```
![image](https://github.com/user-attachments/assets/21d46423-d918-4fd8-9eff-50c7208121e9)


### Step 3 - Install WordPress on your Web Server EC2
1. **Update the repository**
   ```
   sudo yum -y update
   ```
2. **Install wget, Apache and it's dependencies**
```
sudo yum -y install wget httpd php php-mysqlnd php-fpm php-json
```
![image](https://github.com/user-attachments/assets/41452740-ad91-4511-9e78-5daa4cdac5f2)

3. **Start Apache**
```
sudo systemctl enable httpd
sudo systemctl start httpd
```
![image](https://github.com/user-attachments/assets/449820fb-c145-4353-99ee-8fc4d42f018d)

4. **To install PHP and it's dependencies Enable EPEL Repository**
```
sudo dnf install -y epel-release
```
5. **Enable Remi Repository:**
```
sudo dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
```
6. **Enable the PHP Module using dnf**
```
sudo dnf module reset php
sudo dnf module enable php:8.2
```
**Install PHP**
```
sudo dnf install -y php php-cli php-fpm php-json php-common php-mysqlnd php-zip php-gd php-mbstring php-curl php-xml php-pear php-bcmath php-json
```
**Start and enable PHP-FPM service**
```
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
```


**Verify PHP Installation**
```
php -v
```
![image](https://github.com/user-attachments/assets/22a8958e-ea32-4246-ba15-20ff2e699eb9)

**Restart Apache**
```
sudo systemctl restart httpd
```

**Download wordpress and copy wordpress to /var/www/htmlmkdir wordpress**
```
mkdir wordpress
sudo wget http://wordpress.org/latest.tar.gz sudo tar xzvf latest.tar.gz
sudo rm -rf latest.tar.gz cp wordpress/wp-config-sample.php wordpress/wpconfig.php
cp -R wordpress /var/www/html/
```
![Screenshot (498)](https://github.com/user-attachments/assets/0d520b49-48d6-4b4a-ba57-1fe4b53758e3)

**Configure SELinux policies**
```
sudo chown -R apache:apache /var/www/html/wordpress
sudo chcon -t httpd_sys_rw_content_t /var/www/html/wordpress -R
sudo setsebool -P httpd_can_network_connect=1
```


### Step 4 - Install MySQL on the Database server EC2
```
sudo yum update
sudo yum install mysql-server
```
Verify that the server is up and running using sudo systemctl status mysqld, if it is not running, restart the service and enable it so that it will be running even after reboot
```
sudo systemctl restart mysqld
sudo systemctl enable mysqld
sudo systemctl status mysqld
```
![Screenshot (499)](https://github.com/user-attachments/assets/8fc6442d-6f71-4280-8081-482258efdd71)

### Step 5 - Configure Database to work with wordpress
```
sudo mysql
CREATE DATABASE wordpress;
CREATE USER myuser '@'172.31.38.223' IDENTIFIED BY 'mypass';
GRANT ALL ON wordpress.* TO 'myuser'@'172.31.38.223';
FLUSH PRIVILEGE;
SHOW DATABASES;
exit
```
![image](https://github.com/user-attachments/assets/19a4d02b-c188-4e79-8f81-9990512a67f0)

### Step 6 - Configure WordPress to connect to remote database.
Do not forget to open MySQL port 3306 on DB Server EC2. For extra security, allow access to the DB server only from your web Server's IP address, so in the bound configuration specify source as /32
![Screenshot (501)](https://github.com/user-attachments/assets/9539ff6c-9111-47db-840f-f4b3783c8ba1)

1. Install MySQL client and test that you can connect from your web Server to your DB Server by using mysql-client

sudo yum install mysql
sudo mysql -u admin -p -h 172.31.44.175

![Screenshot (502)](https://github.com/user-attachments/assets/6befbb70-d54a-47b2-a257-16f3d6f852e3)

#### Access the WordPress Site
To access the WordPress site, open a web browser and enter the public IP of the web server instance in the address bar.
![Screenshot (507)](https://github.com/user-attachments/assets/9c8aad02-e7a2-4ede-adfc-058ddf8ba986)

Configure and install wordpress
![Screenshot (508)](https://github.com/user-attachments/assets/e81a2cba-9387-45fe-b198-4afabec3a9ba)

**The WordPress Admin Dashboard**
![Screenshot (509)](https://github.com/user-attachments/assets/cfe021b8-3506-43bb-8997-e2352a13a455)

### Conclusion
In this project, I successfully established a WordPress web solution from the ground up. I gained experience in setting up both a physical web server and a database server, as well as configuring the WordPress server. Additionally, I focused on optimizing storage for enhanced performance and scalability by employing disk partitioning techniques. This involved using tools like gdisk, mkfs, and mount to manage disk partitions effectively in Linux.






