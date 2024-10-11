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



