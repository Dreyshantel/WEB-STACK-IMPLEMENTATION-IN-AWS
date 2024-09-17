# Ansible Dynamic Assignments (Include) and Community Roles
In this project we will introduce dynamic assignments by using include module. In the previous project, you can tell that static assignments use import Ansible module. The module that enables dynamic assignments is "include". Hence,
```
import = Static
include = Dynamic
```

When the import module is used, all statements are pre-processed at the time playbooks are parsed. Meaning, when you execute site.yml playbook, Ansible will process all the playbooks referenced during the time it is parsing the statements On the other hand, when include module is used, all statements are processed only during execution of the playbook. Meaning, after the statements are parsed, any changes to the statements encountered during execution will be used.

**Note:** Take note that in most cases it is recommended to use static assignments for playbooks, because it is more reliable. With dynamic ones, it is hard to debug playbook problems due to its dynamic nature. However, you can use dynamic assignments for environment specific variables as we will be introducing in this project

### Introducing Dynamic Asssignments into our structure
In our https://github.com/Dreyshantel/ansible-config-mgt GitHub repository start a new branch and call it dynamic-assignments.
![Screenshot (688)](https://github.com/user-attachments/assets/dbe0589f-9381-4a45-95de-337162d173c8)


Create a new folder named dynamic-assignments. Then inside this folder create a new file name env-vars.yml
![Screenshot (689)](https://github.com/user-attachments/assets/0c1a7915-eed4-4d69-9fdf-3a0b3cad9db3)

Your GitHub should have the following structure:
```
├── dynamic-assignments
│   └── env-vars.yml
├── env-vars
    └── dev.yml
    └── stage.yml
    └── uat.yml
    └── prod.yml
├── inventory
    └── dev
    └── stage
    └── uat
    └── prod
├── playbooks
    └── site.yml
└── static-assignments
    └── common.yml
    └── webservers.yml
```

We will instruct site.yml to include this playbook later. For now, let us keep building up the structure

Since we will be using Ansible to configure multiple enviroments, and each of these enviroments will have certain unique attributes, such as servername, ip-address etc...we will need a way to set values to variables per specific enviroments.

For this reason, we will now create a folder to keep each enviroment's variable file. Therefore, create a new folder "env-vars", then for each enviroment, create new YAML files which we will use to set variables. Your layout should now look like this:

```
├── dynamic-assignments
│   └── env-vars.yml
├── env-vars
    └── dev.yml
    └── stage.yml
    └── uat.yml
    └── prod.yml
├── inventory
    └── dev
    └── stage
    └── uat
    └── prod
├── playbooks
    └── site.yml
└── static-assignments
    └── common.yml
    └── webservers.yml
```


![Screenshot (690)](https://github.com/user-attachments/assets/7af2b608-4080-46c4-8363-c9f5ddc61966)

Now paste the following instructions below into your env-vars.yml file
```
---
- name: collate variables from env specific file, if it exists
  hosts: all
  tasks:
    - name: looping through list of available files
      include_vars: "{{ item }}"
      with_first_found:
        - files:
            - dev.yml
            - stage.yml
            - prod.yml
            - uat.yml
          paths:
            - "{{ playbook_dir }}/../env-vars"
      tags:
        - always
```

![Screenshot (691)](https://github.com/user-attachments/assets/8eeef1c5-c602-41c8-add0-4da39c60b900)

### 3 things to notice here:
1. We used `include_vars` syntax instead of `include`, this is because Ansible developers decided to separate different features of the module. From Ansible version 2.8, the include module is deprecated and variants of include_* must be used. These are:
- include_role
- include_tasks
-  include_vars
 In the same version, variants of import were also introduces, such as: `import_role`, `import_tasks`

3. We made use of a special variables `{{ playbook_dir }}` and `{{ inventory_file }}`. `{{ playbook_dir }}` will help Ansible to determine the location of the running playbook, and from there navigate to other path on the filesystem. `{{ inventory_file }}` on the other hand will dynamically resolve to the name of the inventory file being used, then append .yml so that it picks up the required file within the env-vars folder

4. We are including the variables using a loop. with_first_found implies that, looping through the list of files, the first one found is used. This is good so that we can always set default values in case an environment specific env file does not exist.

### Update site.yml with dynamic assignments
Update site.yml file to make use of the dynamic assignments. (At this point, we cannot test it yet. We are just setting the stage for what is yet to come. So hang on to your hats)

site.yml should now look like this:

```
---
- hosts: all
  name: Include dynamic variables
  become: yes
  tasks:
    - include_tasks: ../dynamic-assignments/env-vars.yml
      tags:
        - always

- import_playbook: ../static-assignments/common.yml

- import_playbook: ../static-assignments/uat-webservers.yml

- import_playbook: ../static-assignments/loadbalancers.yml
```

![Screenshot (692)](https://github.com/user-attachments/assets/8fd5da35-e353-4284-830a-ec0eb3d1f5e3)


### Community Roles
Now it is time to create a role for MySQL database - it should install the MySQL package, create a database and configure users. But why should we re-invent the wheel? There are tons of roles that have already been developed by other open source engineeers out there. These roles are actually production ready, and dynamic to accomodate most of linux flavours. With Ansible Galaxy again, we can simply download a ready to use ansible role, and keep going.


### Download Mysql Ansible Role
You can browse available community roles here We will be using a MySQL role developed by geerlingguy.

Hint: To preserve your your GitHub in actual state after you install a new role - make a commit and push to master your `ansible-config-mgt` directory. Of course you must have git installed and configured on Jenkins-Ansible server and, for more convenient work with codes, you can configure Visual Studio Code to work with this directory. In this case, you will no longer need webhook and Jenkins jobs to update your codes on Jenkins-Ansible server, so you can disable it - we will be using Jenkins later for a better purpose.

### Configure vscode to work with the directory (ansible-config-mgt)
#### Configure SSH for vscode
Step 1: Install the Remote - SSH Extension
  - Open VSCode.
  - Go to the Extensions view by clicking on the Extensions icon in the Activity Bar on the side of the window or press Ctrl+Shift+X.
  - Search for Remote - SSH and click Install on the extension published by Microsoft.

Step 2: Configure SSH
Before connecting, ensure you can SSH into the host using your terminal:
  - Open a terminal (PowerShell on Windows, or terminal on Linux/macOS).
  - Replace username with your remote server username and hostname (or IP address) of the server.

Step 3: Open Folder on Remote Host
After connecting, you’ll be prompted to choose a folder to open on the remote server.
Navigate through the directories to select the folder you want to work on and click Open.

![Screenshot (698)](https://github.com/user-attachments/assets/4ae98fc2-77ba-456f-8b16-539841cb2810)

On `Jenkins-Ansible` server make sure that git is installed with git --version, then go to `ansible-config-mgt` directory and run
```
git init
git pull https://github.com/<your-name>/ansible-config-mgt.git
git remote add origin https://github.com/<your-name>/ansible-config-mgt.git
git branch roles-feature
git switch roles-feature
```
![Screenshot (700)](https://github.com/user-attachments/assets/398282e0-9cda-479e-a14b-b28eeb256483)


### Inside roles directory create your new MySQL role with ansible-galaxy install geerlingguy.mysql
```
ansible-galaxy role install geerlingguy.mysql
```
![Screenshot (701)](https://github.com/user-attachments/assets/5ec756bb-125f-40e1-90fb-5add496bf3de)

**Rename the folder to mysql**
Read README.md file, and edit roles configuration to use correct credentials for MySQL required for the tooling website.

### Create Database and mysql user (roles/mysql/vars/main.yml)
```
mysql_root_password: ""
mysql_databases:
  - name: tooling
    encoding: utf8
    collation: utf8_general_ci
mysql_users:
  - name: webaccess
    host: "" # Webserver subnet cidr
    password: Admin123$
    priv: "tooling.*:ALL"
```
![image](https://github.com/user-attachments/assets/a0082629-2502-4db9-8f25-551e2dab26a1)

### Create a new playbook inside static-assignments folder and name it db-servers.yml , update it with mysql roles.
```
- hosts: db_servers
  become: yes
  vars_files:
    - vars/main.yml
  roles:
    - { role: mysql }
```
### Now it is time to upload the changes into your GitHub:
```
git add .
git commit -m "Commit new role files into GitHub"
git push --set-upstream origin roles-feature
```
![Screenshot (703)](https://github.com/user-attachments/assets/ef72dc5d-2f61-4790-be66-eaeeb4a27731)
![Screenshot (704)](https://github.com/user-attachments/assets/0ce3e102-a5fc-4aae-8fce-d9496606b957)


Now, if you are satisfied with your codes, you can create a Pull Request
![Screenshot (705)](https://github.com/user-attachments/assets/b7e6cba5-3c65-4c85-95df-11280c030353)

Merge it to `Main` branch on GitHub
![Screenshot (706)](https://github.com/user-attachments/assets/146d3cc5-b4d8-49df-ac36-184d5666eeb5)


### Load Balancers roles
We want to be able to choose which Load Balancer to use, `Nginx` or `Apache`, so we need to have two roles respectively:
1. Nginx
2. Apache

With your experience on Ansible so far you can:
- Decide if you want to develop your own roles, or find available ones from the community
- Update both `static-assignment` and `site.yml` files to refer the roles


### Using the Community
![Screenshot (707)](https://github.com/user-attachments/assets/74e9146b-269f-41a7-bf27-436006fda5e7)
![Screenshot (708)](https://github.com/user-attachments/assets/19430013-8956-4aa2-9eab-774a91fac12f)

```
ansible-galaxy role install geerlingguy.nginx

ansible-galaxy role install geerlingguy.apache
```
![Screenshot (709)](https://github.com/user-attachments/assets/e866a454-68db-4150-b833-864d892713b4)


### Rename the installed Nginx and Apache roles
```
mv geerlingguy.nginx nginx

mv geerlingguy.apache apache
```
![Screenshot (710)](https://github.com/user-attachments/assets/54d6cfc2-cd53-4f37-aed4-d7667eb0ef0e)


Your layout should look like this:

![Screenshot (711)](https://github.com/user-attachments/assets/d65700b8-6ae9-45f2-ba00-4e9a5643f6d4)


- Update both static-assignment and site.yml files to refer the roles
 
**Important Hints:**
- Since you cannot use both `Nginx` and `Apache load balancer`, you need to add a condition to enable either one - this is where you can make use of variables.
- Declare a variable in `defaults/main.yml` file inside the `Nginx` and `Apache` roles. Name each variables `enable_nginx_lb` and    `enable_apache_lb` respectively.
- Set both values to false like this `enable_nginx_lb:` false and `enable_apache_lb:` false.
- Declare another variable in both roles load_balancer_is_required and set its value to false as well


#### For Nginx
```
# roles/nginx/defaults/main.yml
enable_nginx_lb: false
load_balancer_is_required: false
```
![Screenshot (712)](https://github.com/user-attachments/assets/0056087d-2a76-43b1-9a70-5b358d2a92b3)


#### For Apache
```
# roles/apache/defaults/main.yml
enable_apache_lb: false
load_balancer_is_required: false
```
![Screenshot (713)](https://github.com/user-attachments/assets/e3d52502-40b1-4dce-b8ad-5f7546c721b7)


### Update assignment
loadbalancers.yml file
```
---
- hosts: lb
  become: yes
  roles:
    - role: nginx
      when: enable_nginx_lb | bool and load_balancer_is_required | bool
    - role: apache
      when: enable_apache_lb | bool and load_balancer_is_required | bool
```
![Screenshot (714)](https://github.com/user-attachments/assets/fa7d90c8-9114-46bb-865e-1dc2cee91b0a0)


### Update site.yml files respectively
```
---
- hosts: all
  name: Include dynamic variables
  become: yes
  tasks:
    - include_tasks: ../dynamic-assignments/env-vars.yml
      tags:
        - always

- import_playbook: ../static-assignments/common.yml

- import_playbook: ../static-assignments/uat-webservers.yml

- import_playbook: ../static-assignments/loadbalancers.yml

- import_playbook: ../static-assignments/db-servers.yml
```
![Screenshot (715)](https://github.com/user-attachments/assets/bb79188a-ae4c-4051-9eff-1bae4e1318f3)

Now, you can use of `env-vars/uat.yml` file to define which loadbalancer to use in UAT enviroment by setting respective enviromental variable to `true`

You will activate load balancer, and enable `nginx` by setting these in the respective enviroment's env-vars file.
```
enable_nginx_lb: true
enable_apache_lb: false
load_balancer_is_required: true
```
![Screenshot (717)](https://github.com/user-attachments/assets/919e887f-736c-472c-9676-3fbf83b01fb9)

# Set up for Nginx Load Balancer
**Update roles/nginx/defaults/main.yml**
Configure Nginx virtual host
```
---
nginx_vhosts:
  - listen: "80"
    server_name: "example.com"
    root: "/var/www/html"
    index: "index.php index.html index.htm"
    locations:
              - path: "/"
                proxy_pass: "http://myapp1"

    # Properties that are only added if defined:
    server_name_redirect: "www.example.com"
    error_page: ""
    access_log: ""
    error_log: ""
    extra_parameters: ""
    template: "{{ nginx_vhost_template }}"
    state: "present"

nginx_upstreams:
- name: myapp1
  strategy: "ip_hash"
  keepalive: 16
  servers:
    - "172.31.35.223 weight=5"
    - "172.31.34.101 weight=5"

nginx_log_format: |-
  '$remote_addr - $remote_user [$time_local] "$request" '
  '$status $body_bytes_sent "$http_referer" '
  '"$http_user_agent" "$http_x_forwarded_for"'
become: yes
```

