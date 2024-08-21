# Tooling Website deployment automation with Continuous Integration
## Continuous Integration
Continuous Integration is software development strategy that increase the speed of development while ensuring the quality of the code that teams deploy.Developers continually commit code in small increment(at least daily,or even several times a day). which is then automatically built and tested before it is merged with the shared repository.

### Task
Enhancing the architecture prepared by adding a Jenkins server, configure a job to automatically deploy source codes changes from Git to NFS server.

### Step 1- Install Jenkins server
1. Launch an ec2 instance:
    - Sign in to AWS management console
    - Navigate to EC2 dashboard.
    - Click on "Launch instance" and choose Ubuntu Server 20.04 LTS as the operating system

![Screenshot (569)](https://github.com/user-attachments/assets/9ef2316f-d76a-47ba-986e-547df21e18a6)


2. Configure Instance Details:
    - Choose instance type, network, subnet, and other required settings.

3. Add Storage
    - Allocate storage space for according to your requirement

4. Add Tags:
    - Optionally, add tags for better organization

5. Configure Security Group
    - Create a new security group or use an existing one.
    - Allow inbound traffic on port 22 (SSH) from your IP address

6. Review and launch:
    - Review the configuration and launch the instance.
    
7. Connect to the Instance
    - Connect to the instance via SSH.


#### Install Jenkins
1. Install JDK (since Jenkins is a Java-based application)
```
sudo apt update    #update package repository
sudo apt install default-jdk-headless
```
![Screenshot (570)](https://github.com/user-attachments/assets/e95c02c5-14d4-4dc1-ac1c-914d91f59bb0)

2. Install Jenkins
Add the jenkins package repository to your system
```
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null

# verify source list
deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/

sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary > \
         /etc/apt/sources.list.d/jenkins.list'

sudo apt update 
sudo apt-get install jenkins
```
![image](https://github.com/user-attachments/assets/b795409b-d714-4b2d-b8eb-20102250410a)

3. Make sure Jenkins is up and running
```
sudo systemctl start jenkins
sudo systemctl enable jenkins
sudo systemctl status jenkins
```
![image](https://github.com/user-attachments/assets/466c8b5f-ae68-4329-bec2-7112733ddba6)

4. By default Jenkins server uses TCP port 8080- open it by creating a new inbound rule in your ec2 security group
![Screenshot (573)](https://github.com/user-attachments/assets/83632136-9b4d-4143-99d4-28a38cbc447f)

6. Perform initial Jenkins setup.
From your browser access http://Public-IP-Address-or-DNS-name:8080. You will be promted to provide a default admin password
![Screenshot (574)](https://github.com/user-attachments/assets/45c6e2f3-574e-4a54-ab0c-24f5df2112e5)

Retrieve it from your server.
```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
Then you would be asked which plugings to install
![Screenshot (575)](https://github.com/user-attachments/assets/3ca25acb-5247-40ea-87ea-8047a3a682cc)
Once the plugin installtion is done- create an admin user and you will get your jenkins server address.



### Step 2- Configure Jenkins to retrieve source codes from Github using webhooks
In this phase, you will configure a simple Jenkins job/project(these two terms can be used interchangeably). This job is triggered by Github and store it locally on Jenkins server.

1. Enable webhooks in your GitHub repository settings
![Screenshot (577)](https://github.com/user-attachments/assets/ac8b121f-7429-4c31-9055-2fa8471f858e)

2. Go to Jenkins Web console, click "New item" and create a "Freestyle project"

