# Ansible Refactoring and Static Assignments (Imports and Roles)
In this project i will keep working with "ansible-config-mgt" repository and make some improvent of the codes. Now i need to refactor the Ansible code, create assignments, and learn how to use the imports functionalities. Imports allows to effectively re-use previously created playbooks in a new playbook - it allows you to organize your task and re-use them when needed.

## Code Refactoring
Refactoring is a general term in computer programming. It means making changes to the source code without changing expected behaviour of the software. The main idea of refactoring is to enhance the code readability, increase maintainability and extensibility, reduce complexity, add proper comments without affecting the logic.

I will move things around a little bit in the code, but the overall state of the infrastructure remains the same.

### Step 1 - Jenkins job enhancement
Before we begin, let us make some changes to our jenkin job.. now every new changes in the codes creates a seperate directory ehich is not very convenient when we want to run some commands from one place. Besides, it consumes spaces on jenkins servers with each subsequent change. Let's enhance it by introducing a new Jenkins project/job- we will require "copy artifact" plugin.

1. Go to your Jenkins-Ansible server and create a new directory called ansible-config-mgt- we will store there all artifacts after each build.
```
sudo mkdir /home/ubuntu/ansible-config-artfifact
```

2. Change permissions to this directory , So jenkins could save files
```
chmod - R 0777 /home/ubuntu/ansible-config-artifact
```

3. Go to Jenkins Web console -> Manage Jenkins -> Manage plugins -> on available tab search for "copy artifact" and install this plugin without restarting Jenkins
![Screenshot (669)](https://github.com/user-attachments/assets/33da7d16-5549-40ae-ac4e-4469f9f0816a)

4. Create a new freestyle project and name it "save_artifacts"
![Screenshot (670)](https://github.com/user-attachments/assets/9815f773-d17e-4360-9b70-6516c705b1f3)

5. This project will be triggered by completion of your existing ansible project. Configure it accordingly:
![Screenshot (671)](https://github.com/user-attachments/assets/08a5490b-3cee-4023-bf4f-f20cbd29a432)
![Screenshot (672)](https://github.com/user-attachments/assets/82448dc1-b8b6-4323-98b4-9e321a2c44ae)

**Note:**  You can configure number of builds to keep in order to save space on the server, for example, you might want to keep only last to or 5 build results. This also applies to the ansible job

6. The main idea of save_artifacts project is to save artifacts into /home/ubuntu/ansible-config-artifact directory. To acheive this, create a build step and choose copy artifacts from other project, specify ansible as source project and /home/ubuntu/ansible-config-artifact as a target directory
![Screenshot (673)](https://github.com/user-attachments/assets/5eba4435-e80d-4991-97ef-4fcea100b3d6)

7. Test your set up by making some change in README.md file inside your ansible-config-mgt repository (right inside main branch). If both Jenkins jobs have completed one after another - you shall see your files inside /home/ubuntu/ansible-config-artifact directory and it will be updated with every commit to your main branch

### Step 2 - Refactor Ansible code by importing other playbooks into site.yml
DevOps philosophy implies constant iterative improvement for better efficiency - refactoring is one of the techniques that can be used.

1. Ceate a new branch and name it branch
```
git branch refactor
git branch -l
```

In previous project, we wrote all tasks in a single playbook common.yml. it will become tedious to use one playbook for multiple servers as the playbook will become messy with commented parts.

2. Within playbooks folder, create a new file and name it site.yml
```
cd playbooks
touch site.yml
ls
```
![Screenshot (674)](https://github.com/user-attachments/assets/e0645264-30af-4f77-b73c-165896479e9c)


This file will now be considered as an entry point into the entire infrastructure configuration. Other playbooks will be included here as a reference. In other words, site.yml will become a parent to all other playbooks that will be developed. Including common.yml that you created previously.


2. Create a new folder in root of the repository and name it static-assignments
```
mkdir static-assignments
ls
```
The static-assignments folder is where all other children playbooks will be stored


3. Move common.yml file into the newly created static-assignments folder
![Screenshot (675)](https://github.com/user-attachments/assets/e2abfbf7-5c3e-4b0e-a712-f5bdf3fd9a94)


4. Inside site.yml file, import common.yml playbook.
```
---
hosts: all
import_playbook: ../static-assignments/common.yml
```
![Screenshot (676)](https://github.com/user-attachments/assets/91dc79ad-c110-4271-8b7d-688f0b6ffde3)


5. Run ansible-playbook command against the dev environment create another playbook under static-assignments and name it common-del.yml. In this playbook, configure deletion of wireshark utility.
```
touch common-del.yml
vi common-del.yml
```

paste the following code in it
```
---
- name: Update web, NFS, and DB servers
  hosts: webservers, nfs, db
  remote_user: ec2-user
  become: yes
  become_user: root
  tasks:
    - name: Delete Wireshark
      yum:
        name: wireshark
        state: removed

- name: Update LB server
  hosts: lb
  remote_user: ubuntu
  become: yes
  become_user: root
  tasks:
    - name: Delete Wireshark
      apt:
        name: wireshark-qt
        state: absent
        autoremove: yes
        purge: yes
        autoclean: yes
```
![Screenshot (677)](https://github.com/user-attachments/assets/bf54b43b-d5de-46a3-8960-d922ef7883a4)


update site.yml with - import_playbook: ../static-assignments/common-del.yml instead of common.yml and run it against dev servers
```
cd /home/ubuntu/ansible-config-mgt/
ansible-playbook -i inventory/dev.yml playbooks/site.yaml
```
![Screenshot (679)](https://github.com/user-attachments/assets/fc486a24-6d29-4d8b-8349-a011c1bd4824)

wireshark has been successfully deleted in all of the servers.
![Screenshot (680)](https://github.com/user-attachments/assets/bbeb4d5d-ae84-49d2-a534-38c05a655a73)



### Step 3 - Configure UAT Webservers with a role "Webserver"
We have our nice and clean dev environment, so let us put it aside and configure 2 new Web Servers as uat.

1. Launch 2 fresh EC2 instances using RHEL 8 image, we will use them as our uat servers, so give them names accordingly - Web1-UAT and Web2-UAT

2. To create a role, you must create a directory called roles/, relative to the playbook file or in /etv/ansible/ directory

- Use an Ansible utility called ansible-galaxy inside ansible-configmgt/roles directory (you need to create roles directory upfront)
```
mkdir roles
cd roles
ansible-galaxy init webserver
```
- Create the directory/files structure manually since we store all our codes in GitHub, it is recommended to create folders and files there rather than locally on Jenkins-Ansible server
  ![Screenshot (681)](https://github.com/user-attachments/assets/15e2c531-4520-43f7-8d08-dc8c22a6f619)

After removing unnecessary files and directories, the roles structure should look like this:
![Screenshot (682)](https://github.com/user-attachments/assets/8ea0ae43-b4f2-4f83-8176-43d9363e2c65)


3. Update your inventory "ansible-config-mgt/inventory/uat.yml" file with IP addresses of your 2 UAT web servers
```
[uat-webservers]
<Web1-UAT-Server-Private-IP-Address> ansible_ssh_user='ec2-user'
<Web2-UAT-Server-Private-IP-Address> ansible_ssh_user='ec2-user'
```

4. In /etc/ansible/ansible.cfg file uncomment roles_path string and provide a full path to your roles directory "roles_path = /home/ubuntu/ansible-config-mgt/roles, so ansible could know where to find configured roles

5. It is time to start adding some logic to the webserver role. Go into tasks directory, and within the main.yml file, start writing configuration tasks to do the following:

- Install and configure Apache (httpd service)
- Clone Tooling website from GitHub https://github.com/Dreyshantel/tooling.git.
- Ensure the tooling website code is deployed to /var/www/html on each of 2 UAT Web servers.
- Make sure httpd service is started
```
---
- name: Setup Apache and deploy a website
  hosts: all
  become: true
  tasks:
    - name: Install Apache
      ansible.builtin.yum:
        name: "httpd"
        state: present

    - name: Install Git
      ansible.builtin.yum:
        name: "git"
        state: present

    - name: Clone a repository
      ansible.builtin.git:
        repo: https://github.com/Dreyshantel/tooling.git
        dest: /var/www/html
        force: yes

    - name: Copy HTML content to one level up
      command: cp -r /var/www/html/html/ /var/www/

    - name: Start Apache service
      ansible.builtin.service:
        name: httpd
        state: started
```
![Screenshot (683)](https://github.com/user-attachments/assets/a0f16331-5aae-4d49-89a1-973a8664322d)


### Step 4 - Reference 'Webserver' role
Within the static-assignments folder, create a new assignment for uat-webservers uat-webserverrrs.yml. This is where you will reference the role.
```
---
- hosts: uat-webservers
  roles:
    - webserver
```

The entry point to our ansible configuration is the site.yml file. Therefore, we need to refer your uat-webservers.yml role inside site.yml. So, we should have this in site.yml
```
---
- hosts: uat-webservers
- import_playbook: ../static-assignments/uat-webservers.yml
```

### Step 5 - Commit and Test
Commit your changes, create a Pull Request and merge them to master branch, make sure webhook triggered two consequent Jenkins jobs, they ran successfully and copied all the files to your JenkinsAnsible server into /home/ubuntu/ansible-config-mgt/ directory

Now run the playbook against your uat inventory and see what happens
**Note:** Before running your playbook, ensure you have tunneled into your Jenkins-Ansible server via ssh-agent
```
cd /home/ubuntu/ansible-config-mgt
ansible-playbook -i /inventory/uat.yml playbooks/site.yml
```
![Screenshot (686)](https://github.com/user-attachments/assets/7ed3645c-113d-48d6-b469-95db98eecc79)

You should be able to see both of your UAT Web servers configured and you can try to reach them from your browser
```
http://13.60.217.155/index.php
```
![Screenshot (687)](https://github.com/user-attachments/assets/3a66f57c-196e-4dd0-9583-7ebf200d89fc)

Your Ansible architecture now look like this:
![image](https://github.com/user-attachments/assets/303d630f-c581-44a9-a848-35f706a4a7e9)



