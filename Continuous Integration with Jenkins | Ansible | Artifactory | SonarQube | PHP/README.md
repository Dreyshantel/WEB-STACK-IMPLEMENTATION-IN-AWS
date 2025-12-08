# EXPERIENCE CONTINUOUS INTEGRATION WITH JENKINS | ANSIBLE | ARTIFACTORY | SONARQUBE | PHP
In this project, you will understand and get hands on experience around the entire concept around CI/CD from applications perspective. To fully gain real expertise around this idea, it is best to see it in action across different programming languages and from the platform perspective too. From the application perspective, we will be focusing on PHP here; there are more projects ahead that are based on Java, Node.js, .Net and Python. By the time you start working on Terraform, Docker and Kubernetes projects, you will get to see the platform perspective of CI/CD in action.

### 13 DevOps Success Metrics
1. **Deployment frequency:** Tracking how often you do deployments is a good DevOps metric. Ultimately, the goal is to do more smaller deployments as often as possible. Reducing the size of deployments makes it easier to test and release. I would suggest counting both production and non-production deployments separately. How often you deploy to QA or pre-production environments is also important. You need to deploy early and often in QA to ensure enough time for testing.

2. **Lead time:** If the goal is to ship code quickly, this is a key DevOps metric. I would define lead time as the amount of time that occurs between starting on a work item until it is deployed. This helps you know that if you started on a new work item today, how long would it take on average until it gets to production.

3. **Customer tickets:** The best and worst indicator of application problems is customer support tickets and feedback. The last thing you want is your users reporting bugs or having problems with your software. Because of this, customer tickets also serve as a good indicator of application quality and performance problems.

4. **Percentage of passed automated tests:** To increase velocity, it is highly recommended that the development team makes extensive usage of unit and functional testing. Since DevOps relies heavily on automation, tracking how well automated tests work is a good DevOps metrics. It is good to know how often code changes break tests.

5. **Defect escape rate:** Do you know how many software defects are being found in production versus QA? If you want to ship code fast, you need to have confidence that you can find software defects before they get to production. Defect escape rate is a great DevOps metric to track how often those defects make it to production.

6. **Availability:** The last thing we ever want is for our application to be down. Depending on the type of application and how we deploy it, we may have a little downtime as part of scheduled maintenance. It is highly recommended to track this metric and all unplanned outages. Most software companies build status pages to track this. Such as this Google Products Status Page

7. **Service level agreements:** Most companies have some service level agreement (SLA) that they promise to the customers. It is also important to track compliance with SLAs. Even if there are no formally stated SLAs, there probably are application non-functional requirements or expectations to be met.

8. **Failed deployments:** We all hope this never happens, but how often do our deployments cause an outage or major issues for the users? Reversing a failed deployment is something we never want to do, but it is something you should always plan for. If you have issues with failed deployments, be sure to track this metric over time. This could also be seen as tracking *Mean Time To Failure (MTTF).

9. **Error rates:** Tracking error rates within the application is super important. Not only they serve as an indicator of quality problems, but also ongoing performance and uptime related issues. In software development, errors are also known as exceptions, and proper exception handling is critical. If they are not handled nicely, we can figure it out while monitoring the rate of errors.

- Bugs – Identify new exceptions being thrown in the code after a deployment
- Production issues – Capture issues with database connections, query timeouts, and other related issuesPresenting error rate metrics like this simply gives greater insights into where to focus attention.


10. **Application usage & traffic:** After a deployment, we want to see if the number of transactions or users accessing our system looks normal. If we suddenly have no traffic or a giant spike in traffic, something could be wrong. An attacker may be routing traffic elsewhere, or initiating a DDOS attack

11. **Application performance:** Before we even perform a deployment, we should configure monitoring tools like Retrace, DataDog, New Relic, or AppDynamics to look for performance problems, hidden errors, and other issues. During and after the deployment, we should also look for any changes in overall application performance and establish some benchmarks to know when things deviate from the norm.

It might be common after a deployment to see major changes in the usage of specific SQL queries, web service or HTTP calls, and other application dependencies. These monitoring tools can provide valuable visualizations like this one below that helps make it easy to spot problems.


12. **Mean time to detection (MTTD):** When problems happen, it is important that we identify them quickly. The last thing we want is to have a major partial or complete system outage and not know about it. Having robust application monitoring and good observability tools in place will help us detect issues quickly. Once they are detected, we also must fix them quickly!

13. **Mean time to recovery (MTTR):** This metric helps us track how long it takes to recover from failures. A key metric for the business is keeping failures to a minimum and being able to recover from them quickly. It is typically measured in hours and may refer to business hours, not calendar hours. 

These are the major metrics that any DevOps team should track and monitor to understand how well CI/CD process is established and how it helps to deliver quality application to the users.


## Simulating a typical CI/CD Pipeline for a PHP Based application
As part of the ongoing infrastructure development with Ansible started from Project 11, you will be tasked to create a pipeline that simulates continuous integration and delivery. Target end to end CI/CD pipeline is represented by the diagram below. It is important to know that both `Tooling and TODO Web` Applications are based on an interpreted (scripting) language (PHP). It means, it can be deployed directly onto a server and will work without compiling the code to a machine language.

The problem with that approach is, it would be difficult to package and version the software for different releases. And so, in this project, we will be using a different approach for releases, rather than downloading directly from git, we will be using Ansible `uri module`.


The problem with that approach is, it would be difficult to package and version the software for different releases. And so, in this project, we will be using a different approach for releases, rather than downloading directly from git, we will be using Ansible uri module.

![image](https://github.com/user-attachments/assets/2288e00a-3b4e-4507-bf64-012f9d454bfb)


### Set Up
To get started, we will focus on these environments initially.

- Ci
- Dev
- Pentest
What we want to achieve, is having Nginx to serve as a reverse proxy for our sites and tools. Each environment setup is represented in the below table and diagrams.

![image](https://github.com/user-attachments/assets/b0df68f4-132e-4073-96c7-0ef97dd0d5b2)

![image](https://github.com/user-attachments/assets/23f7c67b-42e5-4b5e-bb08-5977b06de369)

### Common Best Practices of CI/CD
Before we move on to observability metrics - let us list down the principles that define a reliable and robust CI/CD pipeline:

- Maintain a code repository
- Automate build process
- Make builds self-tested
- Everyone commits to the baseline every day
- Every commit to baseline should be built
- Every bug-fix commit should come with a test case
- Keep the build fast
- Test in a clone of production environment
- Make it easy to get the latest deliverables
- Everyone can see the results of the latest build
- Automate deployment (if you are confident enough in your CI/CD pipeline and willing to go for a fully automated Continuous Deployment)


#### Project Description:
In this project, we will be setting up a CI/CD Pipeline for a PHP based application. The overall CI/CD process looks like the architecture above.

This project is architected in two major repositories with each repository containing its own CI/CD pipeline written in a Jenkinsfile

- **ansible-config-mgt REPO:** This repository contains JenkinsFile which is responsible for setting up and configuring infrastructure required to carry out processes required for our application to run. It does this through the use of ansible roles. This repo is infrastructure specific

- **PHP-todo REPO:** This repository contains jenkinsfile which is focused on processes which are application build specific such as building, linting, static code analysis, push to artifact repository etc.

#### Pre-requisites
We will be making use of AWS virtual machines for this and will require 6 servers for the project which includes:

**Nginx Server:** This would act as the reverse proxy server to our site and tool.

**Jenkins server:** To be used to implement your CI/CD workflows or pipelines. Select a t2.medium at least, Ubuntu 20.04 and Security group should be open to port 8080

**SonarQube server:** To be used for Code quality analysis. Select a t2.medium at least, Ubuntu 20.04 and Security group should be open to port 9000

**Artifactory server:** To be used as the binary repository where the outcome of your build process is stored. Select a t2.medium at least and Security group should be open to port 8081

**Database server:** To server as the databse server for the Todo application

**Todo webserver:** To host the Todo web application.

### Ansible Inventory should look like this:
```
├── ci
├── dev
├── pentest
├── pre-prod
├── prod
├── sit
└── uat
```

ci inventory file
```
[jenkins]
<Jenkins-Private-IP-Address>

[nginx]
<Nginx-Private-IP-Address>

[sonarqube]
<SonarQube-Private-IP-Address>

[artifact_repository]
<Artifact_repository-Private-IP-Address>
```

dev Inventory file
```
[tooling]
<Tooling-Web-Server-Private-IP-Address>

[todo]
<Todo-Web-Server-Private-IP-Address>

[nginx]
<Nginx-Private-IP-Address>

[db:vars]
ansible_user=ec2-user
ansible_python_interpreter=/usr/bin/python

[db]
<DB-Server-Private-IP-Address>
```

pentest inventory file
```
[pentest:children]
pentest-todo
pentest-tooling

[pentest-todo]
<Pentest-for-Todo-Private-IP-Address>

[pentest-tooling]
<Pentest-for-Tooling-Private-IP-Address>
```


**Observations:**

1. You will notice that in the pentest inventory file, we have introduced a new concept `pentest:children` This is because, we want to have a group called pentest which covers Ansible execution against both `pentest-todo` and `pentest-tooling` simultaneously. But at the same time, we want the flexibility to run specific Ansible tasks against an individual group.

2. The `db` group has a slightly different configuration. It uses a RedHat/Centos Linux distro. Others are based on `Ubuntu` (in this case user is ubuntu). Therefore, the user required for connectivity and path to python interpreter are different. If all your environment is based on Ubuntu, you may not need this kind of set up. Totally up to you how you want to do this. Whatever works for you is absolutely fine in this scenario.

This makes us to introduce another Ansible concept called `group_vars`. With group vars, we can declare and set variables for each group of servers created in the inventory file.

For example, If there are variables we need to be common between both `pentest-todo` and `pentest-tooling`, rather than setting these variables in many places, we can simply use the `group_vars` for pentest. Since in the inventory file it has been created as pentest:children Ansible recognizes this and simply applies that variable to both children.


### Configuring Ansible for Jenkins Deployment
1. **Install Jenkins and its dependencies using the terminal**
   
Let's Launch an Ec2 instance on Ubuntu OS for Jenkins server
    - Before installing jenkins and its dependencies, ensure package repository is up to date.
```
sudo apt update
sudo apt upgrade -y
```

### Install Java
Jenkins requires Java to run. install Openjdk 11 or 17, and verify that java is successfully downloaded by checking Java version
```
sudo apt-get update
sudo apt install openjdk-11-jdk

#check java version
java -version
```


### Add Jenkins reppository and import keys
To install the latest stable version of Jenkins, you’ll need to add the Jenkins repository to your system. First, add the Jenkins repository GPG key:

```
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
```


Then, add the Jenkins package repository:
```
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
```

### Install Jenkins
```
sudo apt-get update
sudo apt-get install jenkins -y
```

### Enable and start Jenkins
```
sudo systemctl start jenkins
sudo systemctl enable jenkins
sudo systemctl status jenkins
```

### Allow traffic on jekins default port :8080
Jenkins by default runs on port 8080. If you’re using ufw (Uncomplicated Firewall), allow Jenkins traffic:
```
sudo ufw allow 8080
sudo ufw allow OpenSSH
sudo ufw enable
sudo ufw status
```
![Screenshot (738)](https://github.com/user-attachments/assets/5846025c-1e35-4865-bb38-3bb8b5e44482)
![Screenshot (739)](https://github.com/user-attachments/assets/ba1d8741-1d38-4c69-ae72-5c47a5ba4d39)


### Access Jenkins
Jenkins should now be running and accessible from your web browser. Open your web browser and go to:
```
http://your-server-ip:8080
```


### 2. Install and Open Blue Ocean Jenkins Plugin
Install Blue Ocean which is a modern user interface (UI) for Jenkins, designed to simplify and enhance the experience of building and managing Continuous Integration (CI) and Continuous Delivery (CD) pipelines.

From Jenkins Dashboard click on **Manage Jenkins-> Manage Plugin** and go to `Available tab` to install Blue Ocean
   ![Screenshot (740)](https://github.com/user-attachments/assets/7cb6bb2f-9be3-4c61-a05a-1def16a01ee4)


### Connect Jenkins with GitHub
connect Jenkins to github by navigating to your github account and create an access token to authenticate connection between Jenkins and GitHub
![image](https://github.com/user-attachments/assets/ed717dff-c2b6-42b3-93ec-7cb3c58468ea)


Created a new pipe line... Here is our newly created pipeline. it takes the name of your GitHub repository.
![image](https://github.com/user-attachments/assets/d8d3decb-8770-4b89-985c-d5955f8f3164)


At this point you may not have a `Jenkinsfile` in the Ansible repository, so Blue Ocean will attempt tpo give you some guidance to create one. But we do not need that. We will rather create one ourselves. So, click on Admininstration to exit the Blue Ocean.


**Let us create our `Jenkinsfile`**
Inside the Ansible project, create a new directory `deploy` and start a new file `jenkinsfile` inside the directory.
![image](https://github.com/user-attachments/assets/89eebe9e-a442-4c85-9f45-3eba7314d975)

Add the code snippet below to start building the `Jenkinsfile` gradually. This pipeline currently has just one stage called `Build` and the only thing we are doing is using the `shell script` module to echo `Building Stage`.


```
pipeline {
    agent any


  stages {
    stage('Build')  {
      steps {
        script {
          sh 'echo "Building Stage"'
        }
      }
   }
   }
}
```


Now go back into the Ansible pipeline in Jenkins, and select configure. Scroll down to `Build Configuration` and specify the location of the `Jenkinsfile` at `deploy/Jenkinsfile`

![Screenshot (745)](https://github.com/user-attachments/assets/af73b40b-c38f-407b-b988-0e48a1c0b77d)

Back to the pipeline again, this time click `Build now`

This will trigger a build and you will be able to see the effect of our basic Jenkinsfile configuration by going through the console output of the build.

To really appreciate and feel the difference of Cloud Blue UI, it is recommended to try triggering the build again from Blue Ocean interface.

1. Click on Open Blue Ocean
2. Select your project
3. Click on the play button against the branch

![Screenshot (746)](https://github.com/user-attachments/assets/40e9a5fa-efea-4dca-9fc2-e31aebc1c3d6)

Let us see this in action.

1. Create a new git branch and name ut feature/jenkinspipeline-stages
2. Currently we only have the `Build` stage. Let us add another stage called `Test`. Paste the code snippet below and push the new changes to GitHub

```
   pipeline {
    agent any

  stages {
    stage('Build') {
      steps {
        script {
          sh 'echo "Building Stage"'
        }
      }
    }

    stage('Test') {
      steps {
        script {
          sh 'echo "Testing Stage"'
        }
      }
    }
    }
}
```

Push the new changes to GitHub

To make the new branch to show up in `Jenkins`, we need to tell Jenkins to scan the repository
1. Click on the "Administration" button
2. Navigate to the Ansible project and click on "Scan repository now"
3. Refresh the page and both branches will start building automatically. You can go into Blue Ocean and see both branches there too
![Screenshot (748)](https://github.com/user-attachments/assets/4aa111e2-8d26-4c28-8269-cb87855ba071)

In Blue Ocean, you can now see how the `Jenkinsfile` has caused a new step in the pipeline launch build for new branch
![image](https://github.com/user-attachments/assets/bf84603c-a802-4357-bfbd-6add31018e56)


4. Create a new branch, add more stages into the Jenkins file to simulate below phases. (Just add an echo command like we have in build and test stages)
i.   Package
ii.  Deploy
iii. Clean up
![Screenshot (751)](https://github.com/user-attachments/assets/e83521e9-def6-4f09-a59d-e7c211ad8abb)

5. Verify in Blue Ocean that all the stages are working, then merge your feature branch to the main branch

6. Eventually, your main branch should have a successful pipeline like this in blue ocean
![image](https://github.com/user-attachments/assets/4270ebf2-ab74-4702-8954-731dae3d086f)



## Run Ansible playbook from Jenkins
Now that you have a broad overview of a typical Jenkins pipeline. Let's get the actual Ansible deployment to work by:

1. Install Ansible on Jenkins
```
sudo apt update
sudo apt install ansible -y
ansible -version
```

2. Install Ansible plugin in Jenkins UI
![image](https://github.com/user-attachments/assets/5a975c6b-f915-4edc-bd65-faeb9c70a190)

3. Creating `Jenkinsfile` from scratch. (Delete all you currently have in there and dtart all over to get Ansible to run successfully)
```
pipeline {
    agent any

    environment {
        ANSIBLE_CONFIG = "${WORKSPACE}/deploy/ansible.cfg"
    }

    stages {
        stage('Initial cleanup') {
            steps {
                dir("${WORKSPACE}") {
                    deleteDir()
                }
            }
        }

        stage('Checkout SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/Dreyshantel/ansible-config-mgt.git'
            }
        }

        stage('Prepare Ansible For Execution') {
            steps {
                sh 'echo ${WORKSPACE}'
                sh 'sed -i "3 a roles_path=${WORKSPACE}/roles" ${WORKSPACE}/deploy/ansible.cfg'
            }
        }

        stage('Test SSH Connections') {
            steps {
                script {
                    def allHosts = [
                        'ubuntu@172.31.89.253',
                        'ubuntu@172.31.91.227',
                        'ec2-user@172.31.81.236',
                        'ec2-user@172.31.85.118'
                    ]

                    sshagent(['private-key']) {
                        allHosts.each { host ->
                            sh "ssh -o StrictHostKeyChecking=no ${host} exit"
                        }
                    }
                }
            }
        }

        stage('Run Ansible playbook') {
            steps {
                sshagent(['private-key']) {
                    ansiblePlaybook(
                        become: true,
                        credentialsId: 'private-key',
                        disableHostKeyChecking: true,
                        installation: 'ansible',
                        inventory: '${WORKSPACE}/inventory/dev.yml',
                        playbook: '${WORKSPACE}/playbooks/site.yml'
                    )
                }
            }
        }

        stage('Clean Workspace after build') {
            steps {
                cleanWs(cleanWhenAborted: true, cleanWhenFailure: true, cleanWhenNotBuilt: true, cleanWhenUnstable: true, deleteDirs: true)
            }
        }
    }
}
```

### Note: Ensure that Ansible runs against the Dev environment successfully.
Ensure that the git module in Jenkinsfile is checking out SCM to main branch instead of master (GitHub has discontinued the use of Master due to Black Lives Matter. You can read more here)

Jenkins needs to export the ANSIBLE_CONFIG environment variable. You can put the .ansible.cfg file alongside Jenkinsfile in the deploy directory. This way, anyone can easily identify that everything in there relates to deployment. Then, using the Pipeline Syntax tool in Ansible, generate the syntax to create environment variables to set. https://wiki.jenkins.io/display/JENKINS/Building+a+software+project

Possible issues to watch out for when you implement this

1. Remember that ansible.cfg must be exported to environment variable so that Ansible knows where to find Roles. But because you will possibly run Jenkins from different git branches, the location of Ansible roles will change. Therefore, you must handle this dynamically. You can use Linux Stream Editor sed to update the section roles_path each time there is an execution. You may not have this issue if you run only from the main branch.

2. If you push new changes to Git so that Jenkins failure can be fixed. You might observe that your change may sometimes have no effect. Even though your change is the actual fix required. This can be because Jenkins did not download the latest code from GitHub. Ensure that you start the Jenkinsfile with a clean up step to always delete the previous workspace before running a new one. Sometimes you might need to login to the Jenkins Linux server to verify the files in the workspace to confirm that what you are actually expecting is there. Otherwise, you can spend hours trying to figure out why Jenkins is still failing, when you have pushed up possible changes to fix the error.

3. Another possible reason for Jenkins failure sometimes, is because you have indicated in the Jenkinsfile to check out the main git branch, and you are running a pipeline from another branch. So, always verify by logging onto the Jenkins box to check the workspace, and run git branch command to confirm that the branch you are expecting is there.

If everything goes well for you, it means, the Dev environment has an up-to-date configuration.


### Install Ansible
Confirm your Ansible version with the command below:
```
ansible --version
```

### Configure Ansible on Jenkins
Click on Dashboard > Manage Jenkins > Global Tool Configuration > Add Ansible. Add a name and the path ansible is installed on the jenkins server
![Screenshot (755)](https://github.com/user-attachments/assets/bafcaee1-1ea9-4b83-93e1-2e589d6138cc)

### To ensure jenkins properly connects to all servers, install another plugin called `ssh agent`
![image](https://github.com/user-attachments/assets/7a5cf710-814b-4997-a0e7-5768a616653c)


### Then go to manage jenkins > credentials > global > add credentials
Then follow the steps below:

- Kind: SSH Username with private key
- Scope: Global (Jenkins, nodes, items, all child items, etc)
- ID: private-key (or any ID you prefer)
- Username: Leave it blank or set a default value (e.g., defaultuser) # This is because we are using servers of different username i.e - - Ubuntu and ec2-user. This value won’t be used because the actual usernames will be specified in the Ansible inventory file.
- Private Key: Enter the private key directly

![Screenshot (757)](https://github.com/user-attachments/assets/798786f5-768d-4bb1-a56a-b16a6a5f8e55)
![image](https://github.com/user-attachments/assets/91b90c34-b6f6-4322-b834-3a093cac3283)


### Update inventory/dev.yml by specifying the private IP address of the servers
![image](https://github.com/user-attachments/assets/daadde5c-ff7c-4d3c-af27-d5680a05c300)

### Update the playbook as well
![Screenshot (761)](https://github.com/user-attachments/assets/b9fbecff-fafd-4eb6-9bb7-268601ba35e0)


### Run Ansible against the Dev environment
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/d251e247-fc97-4652-b1a1-701c7aa97f97" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/1dcc7ad2-0e09-4163-ab19-8b05f8c9a3f3" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/46074526-f2f8-420b-b54c-8f700401b373" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/3684cfbf-0ace-49b3-b40f-b26856456663" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/87a7c1fa-3116-4752-8dbe-17248ae34eb6" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/3370b035-3c62-4838-8492-dc4cecc30c72" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/0475ae88-064f-423c-ab5b-06956f7afc2c" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/d70e5eae-7155-4c62-917d-a37c90b96645" />
<img width="959" height="388" alt="image" src="https://github.com/user-attachments/assets/d05bf1c4-2999-4952-9e21-89e08ef0895f" />


### Parameterizing jenkinsfile For Ansible Deployment
To deploy to other enviroment, we will need to use parameters.
1. Update sit inventory with new servers
```
[tooling]
<SIT-Tooling-Web-Server-Private-IP-Address>

[todo]
<SIT-Todo-Web-Server-Private-IP-Address>

[nginx]
<SIT-Nginx-Private-IP-Address>

[db:vars]
ansible_user=ec2-user
ansible_python_interpreter=/usr/bin/python

[db]
<SIT-DB-Server-Private-IP-Address>
```
<img width="957" height="233" alt="image" src="https://github.com/user-attachments/assets/84b47285-d60a-4c81-8121-69f1c18f02eb" />

2. Update Jenkinsfile to introduce parametarization. Below is just one parameter. It has a default value in case if no value is specified at execution. It also has a description so that everyone is aware of its purpose
```
pipeline {
    agent any

    parameters {
      string(name: 'inventory', defaultValue: 'dev',  description: 'This is the inventory file for the environment to deploy configuration')
    }
...
```

<img width="937" height="171" alt="image" src="https://github.com/user-attachments/assets/cb416fd6-df73-4696-9569-93f75c91ac3e" />
3. In the Ansible execution section, remove the hardcoded inventory/dev and replace with `${inventory}
<img width="959" height="199" alt="image" src="https://github.com/user-attachments/assets/fb6c575e-c416-4a4a-9092-ff3413f48dd4" />

- Notice the Build Now has changed to Build with Parameters and this enables us to run differenet environment easily. The default value loads up, but we can now specify which environment we want to deploy the configuration to. Simply type sit and hit Run
<img width="960" height="387" alt="image" src="https://github.com/user-attachments/assets/ed588856-6a8f-4af1-942d-75eee9afb63a" />

- View it from Blue Ocean
  <img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/c11895c7-a695-4ffd-93ed-99c9d12751df" />

4. Add another parameter. This time, introduce tagging in Ansible. You can limit the Ansible execution to a specific role or playbook desired. Therefore, add an Ansible tag to run against webserver only. Test this locally first to get the experience. Once you understand this, update Jenkinsfile and run it from Jenkins.
<img width="959" height="339" alt="image" src="https://github.com/user-attachments/assets/69bf10b7-1fa1-4a9c-a0b5-ba80c2894fe1" />

- Add another parameter to the jenkinsfile. Name the parameter ansible_tags and the default value webserver
<img width="906" height="113" alt="image" src="https://github.com/user-attachments/assets/a5a4b3ef-fa76-48f9-bc65-2e91367a395d" />

- Update the Ansible execution section to prompt for tag
<img width="957" height="166" alt="image" src="https://github.com/user-attachments/assets/dfa1a05b-068e-47bf-9f59-c4ce402961e2" />

- Click on the play button and update the inventory field to sit and the ansible_tags to `webserver`
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/84d7bc0f-6c65-412a-8535-68ab28482e97" />

- Click on `Run` to run the build
 <img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/6229fcfd-728c-4a58-a46a-f269bcb5f302" />

### To avoid hardcoding values in the Jenkinsfile, we can parameterize several more elements. Here are some suggestions:
1. SCM (Source Control Management) URL and Branch: Parameterize the repository URL and the branch to allow flexibility in changing repositories and branches without modifying the Jenkinsfile.
2. SSH Hosts: Parameterize the list of SSH hosts. This will enable you to specify different hosts for different environments or scenarios.
3. Ansible Playbook Path: Parameterize the playbook path to allow different playbooks to be specified.
4. Credentials ID: Parameterize the credentials ID used for SSH and Ansible to support different credentials for different environments.
5. Role Path: The roles path in the Ansible configuration can be parameterized as well.

```
pipeline {
    agent any

    environment {
        ANSIBLE_CONFIG = "${WORKSPACE}/deploy/ansible.cfg"
    }

    parameters {
        string(name: 'inventory', defaultValue: 'dev', description: 'Inventory file for environment')
        string(name: 'branch', defaultValue: 'main', description: 'Branch to checkout')
        string(name: 'repo_url', defaultValue: 'https://github.com/Dreyshantel/ansible-config-mgt.git', description: 'Repository URL')
        text(name: 'ssh_hosts', defaultValue: 'ec2-user@172.31.2.188\nec2-user@172.31.2.111\nec2-user@172.31.2.109\nubuntu@172.31.2.9', description: 'List of SSH hosts, one per line')
        string(name: 'playbook_path', defaultValue: '${WORKSPACE}/playbooks/site.yml', description: 'Path to the Ansible playbook')
        string(name: 'credentials_id', defaultValue: 'private-key', description: 'Credentials ID for SSH and Ansible')    
    }

    stages {

        stage('Initial cleanup') {
            steps {
                deleteDir()
            }
        }

        stage('Checkout SCM') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: "*/${params.branch}"]],
                    userRemoteConfigs: [[
                        url: "${params.repo_url}",
                        credentialsId: 'GitHub-credentials'
                    ]]
                ])
            }
        }

        stage('Prepare Ansible For Execution') {
            steps {
                sh '''
                    echo "WORKSPACE: ${WORKSPACE}"
                    if [ -f "${WORKSPACE}/deploy/ansible.cfg" ]; then
                        echo "Found ansible.cfg — updating roles_path..."
                        sed -i "3 a roles_path=${WORKSPACE}/roles" ${WORKSPACE}/deploy/ansible.cfg
                    else
                        echo "ansible.cfg not found!"
                        exit 1
                    fi
                '''
            }
        }

        stage('Test SSH Connections') {
            steps {
                script {
                    def allHosts = params.ssh_hosts.split('\n')
                    sshagent([params.credentials_id]) {
                        allHosts.each { host ->
                            sh "ssh -o StrictHostKeyChecking=no ${host} exit || echo 'SSH failed for ${host}'"
                        }
                    }
                }
            }
        }

        stage('Run Ansible playbook') {
            steps {
                sshagent([params.credentials_id]) {
                    ansiblePlaybook(
                        become: true,
                        credentialsId: params.credentials_id,
                        disableHostKeyChecking: true,
                        installation: 'ansible',
                        inventory: "${WORKSPACE}/inventory/${params.inventory}",
                        playbook: "${WORKSPACE}/playbooks/site.yml"
                    )
                }
            }
        }

        stage('Clean Workspace after build') {
            steps {
                cleanWs(cleanWhenAborted: true,
                        cleanWhenFailure: true,
                        cleanWhenNotBuilt: true,
                        cleanWhenUnstable: true,
                        deleteDirs: true)
            }
        }
    }

    post {
        always {
            echo 'Final cleanup after build...'
            cleanWs(deleteDirs: true)
        }
    }
}

```
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/10299b2a-64f3-4add-8254-717c944f44c1" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/26b5e687-9dfd-4b80-b4f0-3fea660b77d1" />

## CI/CD Pipline for TODO Application
We already have tooling website as a part of deployment through Ansible. Here we will introduce another PHP application to add to the list of software products we are managing in our infrastructure. The good thing with this particular application is that it has unit tests, and it is an ideal application to show an end-to-end CI/CD pipeline for a particular application. Our goal is to deploy the application onto servers directly from Artifactory rather than from git.
### Todo Project files path
```
/path/to/your/laravel/project
├── app
│   ├── Console
│   ├── Exceptions
│   ├── Http
│   │   ├── Controllers
│   │   ├── Middleware
│   ├── Models
│   ├── Providers
├── bootstrap
│   ├── cache
├── config
│   ├── app.php
│   ├── database.php
│   └── ...
├── database
│   ├── factories
│   ├── migrations
│   ├── seeders
├── public
│   ├── index.php
│   ├── css
│   ├── js
│   ├── ...
├── resources
│   ├── js
│   ├── lang
│   ├── views
│   └── ...
├── routes
│   ├── api.php
│   ├── channels.php
│   ├── console.php
│   ├── web.php
├── storage
│   ├── app
│   ├── framework
│   ├── logs
├── tests
│   ├── Feature
│   ├── Unit
├── vendor
├── .env
├── artisan
├── composer.json
├── composer.lock
├── package.json
├── phpunit.xml
└── webpack.mix.js
```
Our goal here is to deploy the application onto servers directly from Artifactory rather than from git If you have not updated Ansible with an Artifactory role, simply use this guide to create an Ansible role for Artifactory (ignore the Nginx part). Configure Artifactory on Ubuntu 20.04

## Create an Ansible role for Artifactory
### Prerequests
- Ensure port 8082 is opened in artifactory server
### Install Artifactory role using Ansible galaxy collection
```
ansible-galaxy collection install jfrog.platform
```
Update Artifactory role in roles/artifactory/tasks/main.yml to install jfrog Artifactory
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/0b3bfb4e-6ba6-4177-9d32-4acfdf4e9804" />

<img width="943" height="77" alt="image" src="https://github.com/user-attachments/assets/31fa8d92-a395-486a-840d-9491f5ab4f10" />

- Update playbook/site.yml
<img width="947" height="423" alt="image" src="https://github.com/user-attachments/assets/b2944bea-7de4-46d5-b79d-d626f8caa918" />

- Update inventory/ci.yml
<img width="950" height="160" alt="image" src="https://github.com/user-attachments/assets/790b3f7b-ff7b-4f57-83ab-f00abcbb658f" />

### Run the playbook against ci.yml to install jfrog artifactory
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/da63dbd7-5d58-4a6d-b3c6-0db7f62050f8" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/89f4ad8f-9deb-4441-a1cf-6f5bb284a9f8" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/811906c2-6bec-4f59-8f1e-54cf39f9b894" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/5e5d6c26-ede9-4036-81de-ed63169b7c15" />

Access the artifactory GUI on a browser with http://<server public IP address>:8082. Use the default authentication credentials: admin and password to login.

# Phase 1 – Prepare Jenkins
### 1. Fork the repository below into your GitHub account
```
https://github.com/StegTechHub/php-todo.git
```
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/61b5acf2-7eab-44d7-a839-44cb600953e0" />

### 2. On your Jenkins server, install PHP, its dependencies and Composer tool (Feel free to do this manually at first, then update your Ansible accordingly later)
```
sudo apt update

# Install dependancies
sudo apt install -y zip libapache2-mod-php phploc php-{xml,bcmath,bz2,intl,gd,mbstring,mysql,zip}

# Download the installer to the current directory
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"

# Install Composer Globally
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer

# Remove the Installer
php -r "unlink('composer-setup.php');"

# Verify Installation version
php -v
composer -v
```
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/cbd05ff8-dd94-4373-8af2-625fa09f347c" />

The php version installed by the composer is 8.3.6 while the php version of the todo application is 7.4. Ensure to remove the current version and install php 7.4 and its dependencies to avoid error.

### 3. Install Jenkins plugins
- Plot plugin
- Artifactory plugin
- We will use plot plugin to display tests reports, and code coverage information.
- The Artifactory plugin will be used to easily upload code artifacts into an Artifactory server.

### 4. In Jenkins UI configure Artifactory
- Configure the server ID, URL and Credentials, run Test Connection.
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/a09b3a13-a793-4dae-b7e6-23325f05b5be" />


# Phase 2 - Integrate Artifactory repository with Jenkins
1. Create a dummy Jenkinsfile in the repository
2. Using Blue Ocean, create a multibranch Jenkins pipeline

# 3. On the database server, create database and user
- In jenkins server Install my sql client
```
sudo apt install mysql-client -y
```

- Create database and user on database server
```
Create database homestead;
CREATE USER 'homestead'@'%' IDENTIFIED BY 'sePret^i';
GRANT ALL PRIVILEGES ON * . * TO 'homestead'@'%';
````

- Update mysql role in roles/mysql/vars/main.yml
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/22c76515-0f48-4030-84fe-a70f2ebb286f" />

- Run ansible with jenkins to create the database
- Verify that database has been created



<img width="913" height="250" alt="image" src="https://github.com/user-attachments/assets/e28d6875-3622-4179-bf55-10dfbb55b2d1" />

# 4. Update the database connectivity requirements in the file .env.sample
```
APP_ENV=local
APP_DEBUG=true
LOG_LEVEL=debug
APP_KEY=SomeRandomString
APP_URL=http://localhost

DB_HOST=172.31.2.188
DB_DATABASE=homestead
DB_USERNAME=homestead
DB_PASSWORD=sePret^i1
DB_CONNECTION=mysql
BD_PORT=3306

CACHE_DRIVER=file
SESSION_DRIVER=file
QUEUE_DRIVER=sync

REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

MAIL_DRIVER=smtp
MAIL_HOST=mailtrap.io
MAIL_PORT=2525
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
```

# 5. Update Jenkinsfile with proper pipeline configuration
```
pipeline {
    agent any

  stages {

     stage("Initial cleanup") {
          steps {
            dir("${WORKSPACE}") {
              deleteDir()
            }
          }
        }

    stage('Checkout SCM') {
      steps {
            git branch: 'main', url: 'https://github.com/Dreyshantel/php-todo.git'
      }
    }

    stage('Prepare Dependencies') {
      steps {
             sh 'mv .env.sample .env'
             sh 'composer install'
             sh 'php artisan migrate'
             sh 'php artisan db:seed'
             sh 'php artisan key:generate'
      }
    }
  }
}
```
Notice the Prepare Dependencies section

- The required file by PHP is .env so we are renaming .env.sample to .env

- Composer is used by PHP to install all the dependent libraries used by the application

- php artisan uses the .env file to setup the required database objects - (After successful run of this step, login to the database, run show tables and you will see the tables being created for you)

- Update the Jenkinsfile to include Unit tests step
    - Update the Jenkinsfile to include Unit tests step
    - Ensure that all neccesary php extensions are already installed.
    - Run the pipeline build , you will notice that the database has been populated with tables using a method in laravel known as migration and seeding.
 

### Install phpunit
<img width="932" height="239" alt="image" src="https://github.com/user-attachments/assets/3294c0d5-2ef3-4f39-8398-15ff9a669f0f" />

### Install phploc
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/bed9e07f-cf19-4ff3-b175-7c550fdb5de8" />

- Update Jenkinsfile to include Unit tests and then run the pipeline
```
stage('Execute Unit Tests') {
      steps {
             sh './vendor/bin/phpunit'
      }
```


# Phase 3 – Code Quality Analysis
This is one of the areas where developers, architects and many stakeholders are mostly interested in as far as product development is concerned. As a DevOps engineer, you also have a role to play. Especially when it comes to setting up the tools.

For PHP the most commonly tool used for code quality analysis is phploc. Read the article here for more

The data produced by phploc can be ploted onto graphs in Jenkins.

1. Add the code analysis step in Jenkinsfile. The output of the data will be saved in build/logs/phploc.csv file.
```
stage('Code Analysis') {
  steps {
        sh 'phploc app/ --log-csv build/logs/phploc.csv'

  }
}

```

2. Plot the data using plot Jenkins plugin.
This plugin provides generic plotting (or graphing) capabilities in Jenkins. It will plot one or more single values variations across builds in one or more plots. Plots for a particular job (or project) are configured in the job configuration screen, where each field has additional help information. Each plot can have one or more lines (called data series). After each build completes the plots’ data series latest values are pulled from the CSV file generated by phploc

You should now see a Plot menu item on the left menu. Click on it to see the charts. (The analytics may not mean much to you as it is meant to be read by developers. So, you need not worry much about it – this is just to give you an idea of the real-world implementation).

3. Bundle the application code for into an artifact (archived package) upload to Artifactory
```
stage ('Package Artifact') {
    steps {
            sh 'zip -qr php-todo.zip ${WORKSPACE}/*'
     }
    }
```
4. Publish the resulted artifact into Artifactory
```
stage ('Upload Artifact to Artifactory') {
          steps {
            script { 
                 def server = Artifactory.server 'artifactory-server'                 
                 def uploadSpec = """{
                    "files": [
                      {
                       "pattern": "php-todo.zip",
                       "target": "<name-of-artifact-repository>/php-todo",
                       "props": "type=zip;status=ready"

                       }
                    ]
                 }""" 

                 server.upload spec: uploadSpec
               }
            }

        }
```

5. Deploy the application to the dev environment by launching Ansible pipeline
```
stage ('Deploy to Dev Environment') {
    steps {
    build job: 'ansible-project/main', parameters: [[$class: 'StringParameterValue', name: 'env', value: 'dev']], propagate: false, wait: true
    }
  }
```

The build job used in this step tells Jenkins to start another job. In this case it is the ansible-project job, and we are targeting the main branch. Hence, we have ansible-project/main. Since the Ansible project requires parameters to be passed in, we have included this by specifying the parameters section. The name of the parameter is env and its value is dev. Meaning, deploy to the Development environment. Make sure to update artifactory login details in the todo deployment configuration file in ansible-config-mgt project

<img width="885" height="375" alt="image" src="https://github.com/user-attachments/assets/ce248cf8-f9f5-4e7d-8a92-dc910f6cb0f6" />

Make sure zip is install

$ sudo apt install zip -y

But how are we certain that the code being deployed has the quality that meets corporate and customer requirements? Even though we have implemented Unit Tests and Code Coverage Analysis with phpunit and phploc, we still need to implement Quality Gate to ensure that ONLY code with the required code coverage, and other quality standards make it through to the environments.

To achieve this, we need to configure SonarQube – An open-source platform developed by SonarSource for continuous inspection of code quality to perform automatic reviews with static analysis of code to detect bugs, code smells, and security vulnerabilities.

# SONARQUBE INSTALLATION
Before we start getting hands on with SonarQube configuration, it is incredibly important to understand a few concepts:

- Software Quality – The degree to which a software component, system or process meets specified requirements based on user needs and expectations.
- Software Quality Gates – Quality gates are basically acceptance criteria which are usually presented as a set of predefined quality criteria that a software development project must meet in order to proceed from one stage of its lifecycle to the next one.
SonarQube is a tool that can be used to create quality gates for software projects, and the ultimate goal is to be able to ship only quality software code.

Despite that DevOps CI/CD pipeline helps with fast software delivery, it is of the same importance to ensure the quality of such delivery. Hence, we will need SonarQube to set up Quality gates. In this project we will use predefined Quality Gates (also known as The Sonar Way ). Software testers and developers would normally work with project leads and architects to create custom quality gates.

### Install SonarQube on Ubuntu 24.04 With PostgreSQL as Backend Database
Here is a manual approach to installation. Ensure that you can to automate the same with Ansible.

Below is a step by step guide how to install SonarQube 7.9.3 version. It has a strong prerequisite to have Java installed since the tool is Java-based. MySQL support for SonarQube is deprecated, therefore we will be using PostgreSQL.

We will make some Linux Kernel configuration changes to ensure optimal performance of the tool – we will increase vm.max_map_count, file discriptor and ulimit.

### Tune Linux Kernel
This can be achieved by making session changes which does not persist beyond the current session terminal.

```
sudo sysctl -w vm.max_map_count=262144
sudo sysctl -w fs.file-max=65536
ulimit -n 65536
ulimit -u 4096
```

To make a permanent change, edit the file /etc/security/limits.conf and append the 

```
vi /etc/security/limits.conf
sonarqube   -   nofile   65536
sonarqube   -   nproc    4096
```

Before installing, let us update and upgrade system packages:

```
sudo apt-get update
sudo apt-get upgrade
```

Install wget and unzip packages

```
sudo apt-get install wget unzip -y
```

Install OpenJDK and Java Runtime Environment (JRE) 11

```
sudo apt-get install openjdk-11-jdk -y
sudo apt-get install openjdk-11-jre -y
```

Set default JDK – To set default JDK or switch to OpenJDK enter below command:

```
sudo update-alternatives --config java
```

If you have multiple versions of Java installed, you should see a list like below:

```
Selection    Path                                            Priority   Status

------------------------------------------------------------

  0            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1111      auto mode

  1            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1111      manual mode

  2            /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java   1081      manual mode

* 3            /usr/lib/jvm/java-8-oracle/jre/bin/java          1081      manual mode
```

Type "1" to switch OpenJDK 11

Verify the set JAVA Version:

```
java -version
```

### Install and Setup PostgreSQL 10 Database for SonarQube
The command below will add PostgreSQL repo to the repo list:

```
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" >> /etc/apt/sources.list.d/pgdg.list'
```

Download PostgreSQL software

```
wget -q https://www.postgresql.org/media/keys/ACCC4CF8.asc -O - | sudo apt-key add -

```
Install PostgreSQL Database Server

```
sudo apt-get -y install postgresql postgresql-contri
```

Start PostgreSQL Database Server

```
sudo systemctl start postgresql
```

Enable it to start automatically at boot time

```
sudo systemctl enable postgresql
```

Change the password for default postgres user (Pass in the password you intend to use, and remember to save it somewhere)

```
sudo passwd postgres
```

Switch to the postgres user

```
su - postgre
```

Create a new user by typing

```
createuser sonar
```

Switch to the PostgreSQL shell

```
psql
```

Set a password for the newly created user for SonarQube database

```
ALTER USER sonar WITH ENCRYPTED password 'sonar';
```

Create a new database for PostgreSQL database by running:

```
CREATE DATABASE sonarqube OWNER sonar;
```

Grant all privileges to sonar user on sonarqube Database.

```
grant all privileges on DATABASE sonarqube to sonar;
```

Exit from the psql shell and Switch back to the sudo user by running the exit command.

```
\q
exit
```


## Install SonarQube on Ubuntu 24.04 LTS
Navigate to the tmp directory to temporarily download the installation files

```
cd /tmp && sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-7.9.3.zip
```

Unzip the archive setup to /opt directory

```
sudo unzip sonarqube-7.9.3.zip -d /opt
```

Move extracted setup to /opt/sonarqube directory

```
sudo mv /opt/sonarqube-7.9.3 /opt/sonarqube
```

# CONFIGURE SONARQUBE
We cannot run SonarQube as a root user, if you run using root user it will stop automatically. The ideal approach will be to create a separate group and a user to run SonarQube

Create a group sonar

```
sudo groupadd sonar
```

Now add a user with control over the /opt/sonarqube directory

```
sudo useradd -c "user to run SonarQube" -d /opt/sonarqube -g sonar sonar 
sudo chown sonar:sonar /opt/sonarqube -R
```

Open SonarQube configuration file using your favourite text editor (e.g., nano or vim)

```
sudo vim /opt/sonarqube/conf/sonar.properties
```

Find the following lines:

```
#sonar.jdbc.username=
#sonar.jdbc.password=
```

Uncomment them and provide the values of PostgreSQL Database username and password:

```
#--------------------------------------------------------------------------------------------------

# DATABASE

#

# IMPORTANT:

# - The embedded H2 database is used by default. It is recommended for tests but not for

#   production use. Supported databases are Oracle, PostgreSQL and Microsoft SQLServer.

# - Changes to database connection URL (sonar.jdbc.url) can affect SonarSource licensed products.

# User credentials.

# Permissions to create tables, indices and triggers must be granted to JDBC user.

# The schema must be created first.

sonar.jdbc.username=sonar
sonar.jdbc.password=sonar
sonar.jdbc.url=jdbc:postgresql://localhost:5432/sonarqube
```

Edit the sonar script file and set RUN_AS_USER.

```
sudo nano /opt/sonarqube/bin/linux-x86-64/sonar.sh


# If specified, the Wrapper will be run as the specified user.

# IMPORTANT - Make sure that the user has the required privileges to write

#  the PID file and wrapper.log files.  Failure to be able to write the log

#  file will cause the Wrapper to exit without any way to write out an error

#  message.

# NOTE - This will set the user which is used to run the Wrapper as well as

#  the JVM and is not useful in situations where a privileged resource or

#  port needs to be allocated prior to the user being changed.

RUN_AS_USER=sonar
```

Now, to start SonarQube we need to do following: Switch to sonar user

```
sudo su sonar
```

Move to the script directory

```
cd /opt/sonarqube/bin/linux-x86-64/
```

Run the script to start SonarQube

```
./sonar.sh start
```

Expected output shall be as:

```
Starting SonarQube...

Started SonarQube
```

Check SonarQube running status:

```
./sonar.sh status
```

Sample Output below:

```
./sonar.sh status

SonarQube is running (176483).
```

To check SonarQube logs, navigate to /opt/sonarqube/logs/sonar.log directory

```
tail /opt/sonarqube/logs/sonar.log
```

### Output
You can see that SonarQube is up and running

Configure SonarQube to run as a systemd service

Stop the currently running SonarQube service

```
cd /opt/sonarqube/bin/linux-x86-64/
```

Run the script to start SonarQube
```
./sonar.sh stop
```

Create a systemd service file for SonarQube to run as System Startup.
```
 sudo nano /etc/systemd/system/sonar.service
```

Add the configuration below for systemd to determine how to start, stop, check status, or restart the SonarQube service.
```
[Unit]
Description=SonarQube service
After=syslog.target network.target

[Service]
Type=forking

ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop

User=sonar
Group=sonar
Restart=always

LimitNOFILE=65536
LimitNPROC=4096

[Install]
WantedBy=multi-user.target
```

Save the file and control the service with systemctl
```
sudo systemctl start sonar
sudo systemctl enable sonar
sudo systemctl status sonar
```

### We understand how mannually install SonarQube on Ubuntu 20.04 with PostgreSQL as the backend database here let us Launch a instance for sonarqube and configure the environemnt using ansible.

sonarqube installation let us update our ansible config project

1. Set Up Inventory File: Define our target host(s) in an inventory file.
<img width="1368" height="249" alt="image" src="https://github.com/user-attachments/assets/590222c7-66f6-42c2-bb88-0e463af541c8" />

2. Update Ansible Playbook: update a playbook that includes tasks for installing PostgreSQL, creating the SonarQube database and user, installing SonarQube, and configuring it to use PostgreSQL.
<img width="916" height="279" alt="image" src="https://github.com/user-attachments/assets/dfd17c78-3c1e-4f40-b135-289ddafd3d25" />

3. Update Role by adding role fo PostgreSQL and SonarQube
<img width="1064" height="417" alt="image" src="https://github.com/user-attachments/assets/e57906ee-8677-481f-824f-56c01182112d" />

4. Execute the playbook and Access SonarQube To access SonarQube using browser, type server’s IP address followed by port 9000
```
http://server_IP:9000 OR http://localhost:9000
```

Login to SonarQube with default administrator username and password – admin
<img width="1067" height="494" alt="image" src="https://github.com/user-attachments/assets/354a4ca2-ebd1-4176-bc80-762db151cec3" />

Now, when SonarQube is up and running, it is time to setup our Quality gate in Jenkins.

# Configure SonarQube and Jenkins For Quality Gate
- In Jenkins, install SonarScanner plugin

- In Jenkins, install SonarScanner plugin


  
      
