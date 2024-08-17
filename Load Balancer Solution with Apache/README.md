# Load Balancer with Apache 
This project aim at enhancing our tooling website solution by adding a Load Balancer to distribute traffic between underlying servers and allow users to access our website using a single uniform resource locator (URL).

### Prerequisite
1. Two RHEL9 Web servers
2. One Mysql DB Server 
3. One RHEL9 NFS Server

### This project is a continuation of Devops website tooling solution
In Project-7, we configured three web servers, each with its own unique public IP address and DNS name. However, accessing these servers requires clients to remember multiple URLs, which isn't ideal. Managing even three different server addresses can be cumbersome, not to mention the complexity of handling numerous servers like Google's. To simplify access and provide a unified entry point, a Load Balancer (LB) can be implemented. A Load Balancer effectively routes incoming client requests across the web servers, ensuring that traffic is evenly distributed and the servers are utilized efficiently.

![image](https://github.com/user-attachments/assets/b176bb42-477a-4870-8b10-8d58a80946ab)



### Configure Apache As A Load Balancer
1. Create an Ubuntu Server 20.04 EC2 instance and name it Project-8-apache-lb
  - Launch an EC2 Instance:
  - Sign in to the AWS Management Console.
  - Navigate to EC2 Dashboard.
Click on "Launch Instance" and choose Ubuntu Server 20.04 LTS as the operating system.
![Screenshot (555)](https://github.com/user-attachments/assets/54289493-ed7e-48c6-90e9-b1cf62b49483)


**Configure Instance Details:**
     - Choose instance type, network, subnet, and other settings as per your requirements.
   
**Add Storage:**
    - Allocate storage space according to your needs.

**Add Tags:**
   - Optionally, add tags for better organization.

**Configure Security Group:**
   - Create a new security group or use an existing one.
   - Allow inbound traffic on ports 80 (HTTP), 22 (SSH), and 443 (HTTPS) from your anywhere.

**Review and Launch:**
   - Review the configuration and launch the instance.
![Screenshot (557)](https://github.com/user-attachments/assets/68846329-dcd8-484e-a62a-a299c8156275)


SSH into the ec2 instance
![image](https://github.com/user-attachments/assets/417da882-9d9d-427c-8e46-2f79849c31df)


### Install Apache Load Balancer on Project-8-apache-lb server and configure it to point traffic coming to LB to both web servers
Update package repository and install apache2
```
sudo apt update && sudo apt install apache2 -y
```
![image](https://github.com/user-attachments/assets/2da1ef60-948d-4372-abfd-98cbd21e742c)

install the development files for the libxml2 library
```
sudo apt-get install libxm12-dev
```
![Screenshot (560)](https://github.com/user-attachments/assets/5317b6bf-6074-4c1c-855f-deddce3cd0f3)


**Enable following modules:**
```
sudo a2enmod rewrite
sudo a2enmod proxy
sudo a2enmod proxy_balancer
sudo a2enmod proxy_http
sudo a2enmod headers
sudo a2enmod lbmethods_bytraffic
```
![image](https://github.com/user-attachments/assets/5ed247cb-fc8f-4bf0-8268-fdfdcca711fd)

Make sure Apache is up and running
![image](https://github.com/user-attachments/assets/de3a9d5d-c026-466b-a6ac-52e862f34d93)


Configure Load balancing
```
sudo vi /etc/apache2/sites-available/000-defaults.conf
```
Paste the following content into the apache file:
```
<Proxy "balancer://mycluster">
    BalancerMember "http://172.31.33.130:80" loadfactor=5 timeout=1
    BalancerMember "http://172.31.33.130:80" loadfactor=5 timeout=1
    ProxySet lbmethod=bytraffic
</Proxy>

ProxyPreserveHost On
ProxyPass "/" "balancer://mycluster/"
ProxyPassReverse "/" "balancer://mycluster/"
```
![Screenshot (564)](https://github.com/user-attachments/assets/356fa402-7214-4f1a-ac5a-0ce18d0b34d5)

Restart apache server
```
sudo systemctl restart apache2
```

bytraffic balance method will distribute incoming load between your web servers according to current traffic load. We can control in which proportion the traffic must be distributed by loadfactor parameter.

Verifying that our configuration works by accessing the load balancer server public IP addres or public DNS name from the web browser
![Screenshot (565)](https://github.com/user-attachments/assets/913f2ac0-7860-4be8-887e-0efd00285279)

Unmount the NFS server log file 
Open two ssh/putty consoles for both web servers and run the following comand
```
sudo tail -f /var/log/httpd/access_log
```

### Optional Step - Configure Local DNS Names Resolution
when you have multiple servers under your management. The best practice is to configure a local domain name resolution. The easiest way is to use /etc/hosts file, although this approach is not very scalable, but it is very easy to configure and show the concept well

Open this file /etc/hosts/ on your LB server
```
sudo vi /etc/hosts
```

Add 2 records into this file with Local IP address and arbitrary name for both of your Web Servers
```
172.31.46.158 Web1
172.31.33.130 Web2
```
![Screenshot (566)](https://github.com/user-attachments/assets/561ac5d7-07cd-4aaf-951c-6da5b311512e)


Now you can update your LB config with those names instead of IP address
```
BalancerMember http://Web1:80 loadfactor=5 timeout=1
BalancerMember http://Web2:80 loadfactor=5 timeout=1
```
![Screenshot (567)](https://github.com/user-attachments/assets/d330663f-b793-4f1a-857e-1168d71dfc6b)

you can try to curl your web servers from LB locally curl http://web1 or curl://web2- it shall work...this is only internal configuration and it is also local to your LB server, these names will neither be 'resolvable' from other servers internally nor from the Internet.

![Screenshot (568)](https://github.com/user-attachments/assets/103ef4de-863a-4512-aa8f-15a89c38131c)



