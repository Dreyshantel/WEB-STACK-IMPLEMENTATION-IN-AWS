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
![Screenshot (578)](https://github.com/user-attachments/assets/5e9c036c-e7fe-4cf0-a4af-d92528966244)

In configuration of jenkins freestyle project choose Git repository, provide there the link to your Github repository and credentials (user/password) so jenkins could access files and the repository
![Screenshot (579)](https://github.com/user-attachments/assets/ba638c65-c5e7-4ddd-9f57-e2956f1fcfa1)

Save the configuration and let us try to run the build. If everything is configured correctly, the build will be successful
![Screenshot (581)](https://github.com/user-attachments/assets/a3d0c738-f6a5-447f-9b7d-355759d07c16)

You can open the build and check in "Console Output" if it has run successfully. But this build does not produce anything and it runs only when we trigger it manually. let us fix it



3 Click "Configure" your job/project and add these two configurations
Configure triggering the job from GitHub webhook
![Screenshot (582)](https://github.com/user-attachments/assets/fde80db8-f068-422c-b6fe-9aa45dd4a824)

Configure "post-build Actions" to archive all the files- files from resulted build are called 'artifacts".
![Screenshot (583)](https://github.com/user-attachments/assets/2f9e3ea5-78de-4853-a14f-73004c1cf1b5)

Now, go ahead and make some changes in any file in GitHub repository (e.g README.md file) and push the changes to the master branch.
![Screenshot (584)](https://github.com/user-attachments/assets/cb1d20aa-c5e7-409b-82bc-565eb7ab2f94)

You will see that a new build has been launched automatically (by webhook) and you can see its results- artifacts, saved on Jenkins server.
![Screenshot (587)](https://github.com/user-attachments/assets/a7717488-e4dc-4007-a9d7-f01a42e287ae)


i Have now configured an automated Jenkins job that receives files from GitHub by webhook trigger(this method is called 'push' because the changes are being 'pushed' and files transfer is initiated by Github. By default, the artifact are stored on jenkins server locally

```
ls /var/lib/jenkins/jobs/tooling_github/builds/7/archive/
```
![Screenshot (588)](https://github.com/user-attachments/assets/6d55b802-266a-44d1-b32b-2f9a72e7ddb3)



### Step 3 - Configure Jenkins to copy files to NFS server via SSH
Now we have our artifact saved locally on jenkins server, the next step is to copy them to our NFS server to /mnt/apps directory.
Jenkins is a highly extendable application and there are 1400+ plugins available. We will need a plugin that is called "publish OVer SSH"

1. Install "publish over SSH" plugin.
On main dashboard select "Manage Jenkins" and choose "Manage plugins" menu item. 
![Screenshot (589)](https://github.com/user-attachments/assets/4abf0086-032f-4c4c-ba84-d6bbf4ce0fa2)
![Screenshot (592)](https://github.com/user-attachments/assets/299ac96a-8620-46c8-b19c-450b509b649d)


3. Configure the job/project to copy artifact over to NFS server.
On main dashboard select "Manage Jenkins" and choose "Configure system" menu item. Scroll down publish over SSH plugins configuration section and configure it to be able to connect to your NFS server

1. provide a private key (content of .pem file that is used to connect to NFS server via SSH/Putty)
2. Arbitrary name
3. Hostname- private IP address of your NFS server
4. Username - ec2-user (since NFS server is based on RHEL 9)
5. Remote directory - /mnt/apps since our web servers use it as a mounting point to retreive files from the server

Test the configuration and make sure the connection returns success. 
![Screenshot (595)](https://github.com/user-attachments/assets/589ff4b1-d3b7-4867-8c23-6a53505b5961)

Save the configuration, open your jenkins job/project configuration page and add another one "Post-build Action" and click on"Send build artifacts over SSH"

Configure it to send all files produced by the build into our previously define remote directory. In our case we want to copy all files and directories - so we use **. 
![Screenshot (598)](https://github.com/user-attachments/assets/10913687-6184-41d2-af52-aa1d21be25e2)

Save this configuration and go ahead, change something in README.md file in your Github tooling repository.
![Screenshot (599)](https://github.com/user-attachments/assets/9823a697-6530-410d-903f-3d3b365ccffb)

a new line have been added to the README.md file
![Screenshot (600)](https://github.com/user-attachments/assets/36a258c4-730a-4c7e-8220-a238c3219b9e)

the error in the build indicates that there is need for permission to be set for user ec2-user on the NFS server : Ensure the target directory (/mnt) and it's contents on the NFS server has the correct permissions. We might need to change ownership or modify the permissions to allow the Jenkins user to write to it.
```
sudo chown -R ec2-user:ec2-user /mnt/apps
sudo chmod -R 777 /mnt/apps
```
![image](https://github.com/user-attachments/assets/bd9d31c3-3fa4-48b0-93cb-751b227fc447)

**Run the build again from jenkins GUI**
![Screenshot (602)](https://github.com/user-attachments/assets/2dfebd55-6ea9-46ce-ac47-afe4fa91e421)

now the build is successful. To make sure that the files in /mnt/apps have been updated- connect to your NFS server and check README.md file

```
cat /mnt/apps/README.md
```
![Screenshot (603)](https://github.com/user-attachments/assets/04146971-ecfe-4c96-b7a4-f073fc88b31a)

**since the file has been updated, we have just implemented our first Continous Integration solution using Jenkins CI.**
