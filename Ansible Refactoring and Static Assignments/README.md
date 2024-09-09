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

6. The main idea od save_artifacts project is to save artifacts into /home/ubuntu/ansible-config-artifact directory. To acheive this, create a build step and choose copy artifacts from other project, specify ansible as source project and /home/ubuntu/ansible-config-artifact as a target directory
![Screenshot (673)](https://github.com/user-attachments/assets/5eba4435-e80d-4991-97ef-4fcea100b3d6)

7. Test your set up by making some change in README.md file inside your ansible-config-mgt repository (right inside main branch).

If both Jenkins jobs have completed one after another - you shall see your files inside /home/ubuntu/ansible-config-artifact directory and it will be updated with every commit to your main branch
