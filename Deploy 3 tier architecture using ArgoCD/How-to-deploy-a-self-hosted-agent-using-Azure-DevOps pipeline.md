# CI/CD project implementation
## Deploy 3 tier architecture voting Application using ArgoCD and Azure DevOps Pipeline
<img width="700" height="334" alt="image" src="https://github.com/user-attachments/assets/278bea0a-529f-4e98-a83f-23bf9921a662" />

## Table of Content
**Stage one: Continuous Integration**
1. Clone and Deploy the App locally using Docker-compose
2. Create an Azure DevOps project and import the repo
3. set up self-hosted Agent for the pipeline
4. Write a CI pipeline script for each Microservices

**Stage two: Continuous Delivery**
1. Create an Azure managed Kubernetes cluster (AKS)
2. Install AKS Cli and set up AKS for use
3. Install and Configure ArgoCD
4. Write a Bash script that update the pipeline image on K8s manifest
5. Create an ACR ImagePullSecret

   <img width="700" height="655" alt="image" src="https://github.com/user-attachments/assets/b69bc9aa-dd3b-41f8-b014-e247e4982d48" />

The Voting App is a microservices-based application, typically written in Python and Node.js, with the following components:

1. Voting Frontend (Python/Flask): Users cast votes via a simple web interface.
2. Vote Processor Backend (Node.js/Express): Receives and processes the votes.
3. Redis Database (Redis): Temporarily stores votes for fast access.
4. Worker (Python/Flask): Processes votes from Redis and updates the final count.
5. Results Frontend (Python/Flask): Displays real-time voting results.
6. PostgreSQL Database (PostgreSQL): Stores the final vote count.

How It Works:
The user votes on the frontend, the vote is processed and stored in Redis, then the worker updates the final count on postgreSQL, and the result are shown on the result page. 


# 1. Clone and depoy the App locally usind Docker-compose
To do this, we would have to create an Azure VM in Azure portal to test our App locally using Docker-compose.

a. Create an Ubuntu linux VM on Azure and login.

click [here](https://learn.microsoft.com/en-us/azure/virtual-machines/linux/quick-create-portal?tabs=ubuntu) if you are not familiar with the steps needed in creating linux VM in Azure. When the VM is created, navigate to the folder and `ssh`into the VM using the command `ssh -i <your-key-name> azureuser@<your-ip-name>`

**Note:** Make sure port 22 is open when creating the VM. (It usually opens by default for linux systems.)

b. Update the virtual machine by running the command:
```
sudo apt-get update
```

c. Install Docker and Docker-compose.
To be able to test our app locally, we need to install docker and docker-compose. This is needed for us to run `docker-compose up -d` command.
**Note:** docker0-compose is used to define and run multi-container applications such as microservices.
```
1. Install required packages
sudo apt-get install \
  ca-certificates \
  curl \
  gnupg \
  lsb-release

2. Add Docker's Official GPG Key
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

3. Set Up the Stable Repository
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

4. Install Docker Engine
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io -y

5. Install Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/download/$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep -oP '"tag_name": "\K(.*)(?=")')/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

6. Manage Docker as a Non-Root User and refresh it
sudo usermod -aG docker $USER
newgrp docker

7. Check versions
sudo docker --version
docker-compose --version

8. Check you can docker command without sudo
docker ps
```

**Note:** `docker ps` output should be empty since no containers are running yet.


d. Clone the Git repository
Click [here](https://github.com/dockersamples/example-voting-app) to clone the voting app git repo

<img width="1481" height="364" alt="image" src="https://github.com/user-attachments/assets/65e5c2c9-42c4-4b9f-a28d-aa7f64960687" />

Run the command to spin up all the containers at once
```
docker-compose up -d
```
<img width="1592" height="416" alt="image" src="https://github.com/user-attachments/assets/c92f4f83-b4a4-4900-bfb4-a514bff992e3" />

This output shows that all the containers are up and running
<img width="1480" height="323" alt="image" src="https://github.com/user-attachments/assets/2dbc9585-c780-4fb7-9583-dd6004dad68d" />


From the output we can see the App container is mapped on port 8080 ; We can view the App in two (2) ways; to view the App do the following.

a. Run curl http://localhost:8080 to view the App on the terminal

b. Copy the VM public address and type into the browser http://vm-public-ip>:8080 to view the App.
<img width="1899" height="948" alt="image" src="https://github.com/user-attachments/assets/d63cd6c1-c87b-4c34-990f-a751a9edec4b" />

# Step 2: Create an Azure DevOps Project and Import the repo

