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

We will instruct site.yml to include this playbook later. For now, let us keep building up the structure

Since we will be using Ansible to configure multiple enviroments, and each of these enviroments will have certain unique attributes, such as servername, ip-address etc...we will need a way to set values to variables per specific enviroments.

For this reason, we will now create a folder to keep each enviroment's variable file. Therefore, create a new folder "env-vars", then for each enviroment, create new YAML files which we will use to set variables. Your layout should now look like this:

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
1. We used include_vars syntax instead of include, this is because Ansible developers decided to separate different features of the module. From Ansible version 2.8, the include module is deprecated and variants of include_* must be used. These are: include_role include_tasks include_vars In the same version, variants of import were also introduces, such as: import_role, import_tasks

2. We made use of a special variables {{ playbook_dir }} and {{ inventory_file }}. {{ playbook_dir }} will help Ansible to determine the location of the running playbook, and from there navigate to other path on the filesystem. {{ inventory_file }} on the other hand will dynamically resolve to the name of the inventory file being used, then append .yml so that it picks up the required file within the env-vars folder

3. We are including the variables using a loop. with_first_found implies that, looping through the list of files, the first one found is used. This is good so that we can always set default values in case an environment specific env file does not exist.

### Update site.yml with dynamic assignments
Update site.yml file to make use of the dynamic assignments. (At this point, we cannot test it yet. We are just setting the stage for what is yet to come. So hang on to your hats)

site.yml should now look like this:

```
---
- hosts: all
- name: Include dynamic variables
tasks:
import_playbook: ../static-assignments/common.yml
include: ../dynamic-assignments/env-vars.yml
tags:
- always
- hosts: webservers
- name: Webserver assignment
import_playbook: ../static-assignments/webservers.yml
```

![Screenshot (692)](https://github.com/user-attachments/assets/8fd5da35-e353-4284-830a-ec0eb3d1f5e3)


### Community Roles
Now it is time to create a rolw for MySQL database - it should install the MySQL package, create a database and configure users. But why should we re-invent the wheel? There are tons of roles that have already been developed by other open source engineeers out there. These roles are actually production ready, and dynamic to accomodate most of linux flavours. With Ansible Galaxy again, we can simply download a ready to use ansible role, and keep going.

### Download MySQL Ansible Role
We will be using a MySQL role developed by geerlingguy
```
ansible-galaxy role install geerlingguy.mysql
```
