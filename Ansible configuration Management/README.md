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


Now, ssh into your Jenkins-Ansible server using ssh-agent
```
ssh ubuntu@13.60.174.1
```
![Screenshot (644)](https://github.com/user-attachments/assets/d501888b-9e61-4a0e-aa57-0817290ea9af)


Update your inventory/dev.yml file with this snippet of code:
```
[nfs]
172.31.47.28 ansible_ssh_user=ec2-user
[webservers]
172.31.46.158 ansible_ssh_user=ec2-user
172.31.33.130 ansible_ssh_user=ec2-user
[db]
172.31.44.175 ansible_ssh_user=ubuntu
[lb]
172.31.35.152 ansible_ssh_user=ubuntu
```
![Screenshot (645)](https://github.com/user-attachments/assets/d7cb54bc-c0ce-4714-9cb1-3ac1d1ac0a5f)


### Step 5 - Create a Common Playbook
It is time to start giving Ansible the instructions on what you need to be performed on all servers listed in inventory/dev In common.yml playbook you will write configuration for repeatable, re-usable, and multi-machine tasks that is common to systems within the infrastructure. Update playbooks/common.yml file with following code:

```
---
- name: Update web, NFS, and DB servers
  hosts: webservers, nfs, db
  become: yes
  tasks:
    - name: Ensure Wireshark is at the latest version
      yum:
        name: wireshark
        state: latest

- name: Update LB server
  hosts: lb
  become: yes
  tasks:
    - name: Update apt repository cache
      apt:
        update_cache: yes
      
    - name: Ensure Wireshark is at the latest version
      apt:
        name: wireshark
        state: latest

```
![Screenshot (646)](https://github.com/user-attachments/assets/d0046a9e-840f-4b41-b5db-07c50cfcb2f4)

The Ansible playbook updates Wireshark on web, NFS, and DB servers using yum, and on the LB server using apt lets update this playbook with following tasks: Create a directory and a file inside it Change timezone on all servers Run some shell script

### Step 6 - Update GIT with the latest code
Now all of your directories and files live on your machine and you need to push changes made locally to GitHub. In many organisations there is a development rule that do not allow to deploy any code before it has been reviewed by an extra pair of eyes - it is also called "Four eyes principle". Now you have a separate branch, you will need to know how to raise a Pull Request (PR), get your branch peer reviewed and merged to the master branch. Commit your code into GitHub:

1. Use git commands to add, commit and push your branch to GitHub.
```
git add .
git status
git commit -m "ansible playbook"
```

 push to remote repository branch
```
git push origin feature/ans-prj-01
```

2. Create a Pull Request (PR) click on compare and pull request.
![Screenshot (647)](https://github.com/user-attachments/assets/99a8013c-b685-4a65-8ef6-110944ea5b6f)

3. Let's put on a hat of a developer and act as a reviewer
![Screenshot (648)](https://github.com/user-attachments/assets/e1873a20-1dd3-4ff5-a3f1-3c23b64ddcc8)

4. After revison, and merge the pull request
![Screenshot (651)](https://github.com/user-attachments/assets/adccebbb-eadb-4983-9a01-7f22f04ad512)

5.  Head back on your terminal, checkout from the feature branch into the master, and pull down the latest changes
```
git checkout -b main
git pull
```
![Screenshot (653)](https://github.com/user-attachments/assets/df3d15e6-6e74-4b48-8f4a-e8874a563ea0)

Once your code changes appear in master branch - Jenkins will do its job and save all the files (build artifacts) to /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/ directory on Jenkins-Ansible server

### Step 7 - Run the first Ansible test.
Now, it is time to execute ansible-playbook command and verify if our playbook actually works: 1.Setup your VSCode to connect to your instance 

2. run the code below in ansibleconfig-mgt. directory
**Note:** Make sure you are in the ansible configuration management directory before running the code
```
ansible-playbook -i inventory/dev.yml playbooks/common.yml
```
![Screenshot (665)](https://github.com/user-attachments/assets/7499cb76-0faa-4993-b214-4a076a8270eb)

You can go to each servers and check if wireshark has been installed using the "wireshark --version" command

![image](https://github.com/user-attachments/assets/726a7e72-aecc-4f97-a475-a65c4ae15321)
![image](https://github.com/user-attachments/assets/32c8feb6-e879-4635-8bc9-564b99cafe28)

Now your ansible architecture now look like this:
![image](https://github.com/user-attachments/assets/b786926e-9e10-444d-aace-7792edc0c6d3)




