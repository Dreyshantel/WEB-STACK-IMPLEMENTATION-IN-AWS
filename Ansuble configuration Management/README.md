# Ansible Configuration Management (Automate Project 7 to 10)- 101.
This Project will Automate majority of the routine tasks with Ansible Configuration Management.

### Ansible Client as a Jump Server (Bastion Host):
A Jump Server (sometimes also referred as Bastion Host) is an intermediary server through which access to internal network can be provided. we will develop the use of ansible script to simulate the use jump/ bastion host to access our web servers. It provides better security and reduce attack surface

### Tasks
1. Install and configure Ansible client to act as a Jump Server/Bastion Host
2. Create a simple Ansible playbook to automate servers configuration.

## The Architecture
![image](https://github.com/user-attachments/assets/657c18b3-26e5-4550-b63e-5cbe0be276de)


### Step 1 - Install and Configure Ansible on EC2 Instance.
1. Update the Name tag on your Jenkins EC2 Instance to Jenkins-Ansible. We will use this server to run playbooks.

2. In your GitHub account create a new repository and name it ansibleconfig-mgt.
![Screenshot (640)](https://github.com/user-attachments/assets/3851427c-dd26-40d5-9bee-19bc5c429502)


3. Install Ansible
```
sudo apt update
sudo apt install ansible
```

check if ansible is installed
```
ansible --version
```
![image](https://github.com/user-attachments/assets/55fd5e8e-4df3-49cb-b340-94be21777f92)

4. Configure Jenkins build job to archive your repository content every time you change it.
setup a webhook on ansibleconfig-mgt repository on github
![image](https://github.com/user-attachments/assets/28b652ca-48c1-4148-bcd2-9cf7ca7823d3)

Create a new Freestyle project ansible in Jenkins and point it to your 'ansible-config-mgt' repository
![image](https://github.com/user-attachments/assets/12a3b837-8f9e-456f-a687-539eb44819c0)

Configure a Post-build job to save all (**) files
![Screenshot (636)](https://github.com/user-attachments/assets/2f197bdf-cf69-4611-a654-545d2f1f42fb)
![Screenshot (638)](https://github.com/user-attachments/assets/ebc8c414-2fc9-4551-ab3e-b0d2ee96db56)

Test your setup by making some change in README.md file in main branch.The file is saved alraedy in /var/lib/jenkins/jobs/ansible/builds/3/archive/
```
ls /var/lib/jenkins/jobs/ansible/builds/3/archive/
```
**Note:** Trigger Jenkins project execution only for main (or master) branch.


### Step 2 - Prepare your development environment using Visual Studio Code
1. Install and launch visual studio code
2. After you have successfully installed VSC, configure it to connect to your newly created GitHub repository.

  - install git pull request extension
  - create a new folder nam ansibleconfig-mgt open it with visual studio code
  - clone the repository from github into the folder using
```
git clone https://github.com/Dreyshantel/ansible-config-mgt
```


### Step 3 - Begin Ansible Development.
1. In your ansible-config-mgt GitHub repository, create a new branch that will be used for development of a new feature.
![Screenshot (640)](https://github.com/user-attachments/assets/f76df0c9-087b-41b9-a6b6-cdeba8c8aa21)

2. Checkout the newly created feature branch to your local machine and start building your code and directory structure run the command below on vscode terminal
```
git branch -m feature/ans-prj-01
```
3. Create a directory and name it playbooks - it will be used to store all your playbook files
```
mkdir playbooks
```
4. Create a directory and name it inventory - it will be used to keep your hosts organised.
```
mkdir inventory
```
5. Within the playbooks folder, create your first playbook, and name it common.yml
```
cd playbooks
touch common.yml
```
6. Within the inventory folder, create an inventory file () for each environment (Development, Staging Testing and Production) dev, staging, uat, and prod respectively. These inventory files use .ini languages style to configure Ansible hosts
```
cd /ivnventory
touch dev staging uat prod
```


### Step-4 Setup an Ansible inventory
An Ansible inventory file defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operates

Save the below inventory structure in the inventory/dev file to start configuring your development servers. Note: Ansible uses TCP port 22 by default, which means it needs to ssh into target servers from Jenkins-Ansible host
  - Confirm the key has been added with the command below, you should see the name of your key.


```
eval $(ssh-agent -s)
ssh-add "Timi keypair.pem
ssh-add -l
```
![image](https://github.com/user-attachments/assets/d41ab973-0200-4d10-b2c1-03502535cd31)
