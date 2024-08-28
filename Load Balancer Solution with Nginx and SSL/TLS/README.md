# Load Balancer Solution with Nginx and SSL/TLS
When data is moving between a client and a web server over the internet- it passes through multiple network devices and, if the data is not encrypted, it can be relatively easy intercepted by someone who has access to the intermediate equipment. This kind of information security threat is called Man-in-the-middle (MITM) attack.


SSL/TLS is a security technology that protects connection from MITM attack by creating an encrypted session between browser and web server. It uses digital certificates to identify and validate a website. A browser reads the certificate issued by a Certificate Authority to make sure that the website is registered in the (CA) so it can be trusted to establish a secured connection

## Task
This project consist of two parts:
1. Configure Nginx as a Load Balancer
2. Register a new domain name and configure secured connection using SSL/TLS certificates

![image](https://github.com/user-attachments/assets/69b5e97d-bc36-4529-8daa-178101ad2468)


### Step 1 - Configure Nginx as a Load Balancer
1. Create an EC2 VM based on Ubuntu Server 20.04 LTS
![Screenshot (604)](https://github.com/user-attachments/assets/8453905e-8329-4bb6-82a4-d4c3a9fad823)

Open Tcp port 80 for HTTP connections, also open TCP port 433 - this port is used for secured HTTPS connections
![Screenshot (605)](https://github.com/user-attachments/assets/758deb27-91c8-4f7f-bc2b-ceafeb705b9e)

SSH into the Nginx LB EC2 instance
![Screenshot (607)](https://github.com/user-attachments/assets/95188a55-7915-4e01-bd52-0381147caa45)

2. Update /etc/hosts file for local DNS with web servers names (e.g web1 and web2) and their local IP addresses
![image](https://github.com/user-attachments/assets/2e9d704b-52a1-4da4-80e2-85ccb1639f04)

3. Install and configure Nginx as a load balancer to point traffic to the resolvable DNS names of the web servers

Update the instance and install Nginx
```
sudo apt update
sudo apt install nginx
```
![Screenshot (608)](https://github.com/user-attachments/assets/0a2b220b-2ba6-4707-8896-4e27b07054ba)

Configure Nginx LB using web servers names defined in /etc/hosts
```
sudo vi /etc/nginx/nginx.conf
```
Insert following configuration into htp section
```
}


        upstream myproject {
           server web1 weight=5;
           server web2 weight=5;
         }

        server {
            listen 80;
            server_name www.domain.com
            location / {
              proxy_pass http://myproject;
            }
         }

# comment out this line
#    include /etc/nginx/sites-enabled/*;

```

Check if the configuration syntax is correct
![image](https://github.com/user-attachments/assets/f6fa841d-01e7-41dd-aff4-8f8e70794b49)


Restart Nginx and make sure the service is up and running
```
sudo systemctl restart nginx
suido systemctl status nginx
```
![Screenshot (611)](https://github.com/user-attachments/assets/cb949c65-f5d1-4d26-917d-b87f53b391ff)


### Step 2 - Register a new domain name and configure secured connection using SSL/TLS certificates

In order to get a valid SSL certificate - you need to register a new domain name, you can do it using any Domain name registrar - a company that manages reservation of domain names. The most popular ones are: Godaddy.com, Domain.com, Bluehost.com.

1. Register a new domain name with any registrar of your choice in any domain zone (e.g. .com, .net, .org, .edu, .info, .xyz or any other) freedns.afraid.org is used in this project to get a free domain name
![Screenshot (614)](https://github.com/user-attachments/assets/9e1f34e6-405d-4e10-809c-3059b125d046)

2. Assign an elastic IP to your Nginx Lb server and associate your domain name with this elastic IP
![Screenshot (615)](https://github.com/user-attachments/assets/f339effa-b1e4-4401-aeae-b4f41c5a21c6)

Associate your elastic IP to your ec2 server
![Screenshot (616)](https://github.com/user-attachments/assets/f56cd4a3-1b90-4d6d-b01f-765b4ae86064)

3. Update a record in your registrar to point to Nginx LB using elastic IP address
![Screenshot (624)](https://github.com/user-attachments/assets/f698dd5e-cbfd-46c6-8443-e149bb3de150)


Check that your Web Servers can be reached from your browser using newdomain name using HTTP protocol - http://tooling.twilightparadox.com

Confirming Domain name using DNS checker
![Screenshot (623)](https://github.com/user-attachments/assets/54c7788a-3a5f-49ab-86a4-16c5824e2111)



4. Configure Nginx to recognize your new domain name
Update your nginx.conf with www.strangled.net instead of www.domain.com
```
sudo vi /etc/nginx/nginx.conf
```

5. Install certbot and request for an SSL/TLS certificate. Make sure "snapd" service is active and running
```
sudo systemctl status snapd
```
![Screenshot (621)](https://github.com/user-attachments/assets/48b3c59f-e950-497a-a289-3d761ddc24cc)


Install certbot
```
sudo snap install --classic certbot
```
![image](https://github.com/user-attachments/assets/53a59773-5719-4080-ad49-148fe8d03015)

Request your certificate (just follow the certbot instructions - you will need to choose which domain you want your certificate to be issued for, domainname will be looked up from nginx.conf file so make sure you have updated it on step 4)
```
sudo ln -s /snap/bin/certbot /usr/bin/certbot
sudo certbot --nginx
```
![image](https://github.com/user-attachments/assets/ab3c1cdb-e46e-4600-9b2a-831d290e5c77)

Test secured access to your Web solution by trying to reach https://tooling.twilightparadox.com
![Screenshot (628)](https://github.com/user-attachments/assets/edf4f998-d6a4-4968-99c7-89ae3c3ca864)


6. Set up periodical renewal of your SSL/TLS certifficate
By default, let's encrypt certificate is valid for 90 days, so it is recommendable to renew it at least every 60 days or more frequently
   - You can test renewal command in dry-run mode
```
sudo certbot renew --dry-run
```
![image](https://github.com/user-attachments/assets/eb83683a-2860-468e-a1af-b21612e2ff22)

Best practice is to have a scheduled job that to run renew command periodically. Lets us configure a cronjob to run the command twice a day. To do so let's edit crontab file with the following command:
```
crontab -e
```
Add the following line:
```
* */12 * * * root /usr/bin/certbot renew > /dev/null 2>&1
```
![Screenshot (630)](https://github.com/user-attachments/assets/25c8f21b-5ec5-47a7-a159-04e586e24a42)
You can always change the interval of this cronjob if twice a day is too often by adjusting schedule expression
