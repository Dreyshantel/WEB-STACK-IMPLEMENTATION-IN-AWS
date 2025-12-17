# AWS CLOUD SOLUTION FOR 2 COMPANY WEBSITES USING A REVERSE PROXY TECHNOLOGY
## INTRODUCTION

This project demostrates how a secure infrastructure inside AWS VPC (Virtual Private Cloud) network is built for a particular company, who uses [WordPress CMS](https://wordpress.com/) for its main business website, and a [Tooling Website](https://github.com/Dreyshantel/tooling) for their DevOps team. As part of the company’s desire for improved security and performance, a decision has been made to use a [reverse proxy technology from NGINX](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/) to achieve this. The infrastructure will look like following diagram:

<img width="1324" height="1414" alt="architecture(1)" src="https://github.com/user-attachments/assets/d29e4924-bbf8-4992-8391-f8b056b4ce17" />

## Starting Off Your AWS Cloud ProjectStarting Off Your AWS Cloud Project
There are few requirements that must be met before you begin:
1. Properly configure your AWS account and Organization Unit [watch how to do this here](https://www.youtube.com/watch?v=9PQYCc_20-Q)
-Create an AWS master account. (Also Known as Root account)
Go to AWS console, and navigate to Services > All Services > Management & Governance > AWS Organizations
<img width="960" height="517" alt="image" src="https://github.com/user-attachments/assets/0588b9c7-81f6-4f09-9087-08ea9b2e1ae5" />

  Within the Root account, create a sub-account and name it DevOps. (You will need another email address to complete this)
- Navigate and click on Add an aws account and name the account (DevOps)
  <img width="960" height="517" alt="image" src="https://github.com/user-attachments/assets/0a402e5a-0da1-4c10-99ea-cfa82d8cfdc6" />
  <img width="960" height="517" alt="image" src="https://github.com/user-attachments/assets/d4141dae-b467-4aec-bd9d-e6705691a270" />

    - Within the root account, create an AWS Organization Unit (OU). Name it `Dev`. (We will launch the Dev resources in there)
    
  From the AWS Organization page, Click on root > Action > Create new
  <img width="960" height="517" alt="image" src="https://github.com/user-attachments/assets/df5e9bf5-c7a3-484c-a0f5-6507b271d955" />

 - Move the DevOps account into the Dev OU.
  select the account to move, then click action > move, then select the OU to move the account to and click move AWS account.
<img width="960" height="517" alt="image" src="https://github.com/user-attachments/assets/6ae5a65b-5aa5-4bfe-b69a-335eeb228852" />

 - Login to the newly created AWS account using the new email address.
 <img width="957" height="284" alt="image" src="https://github.com/user-attachments/assets/4ec5c166-269f-4348-a390-1f74f83ee781" />

2. Create a free domain name for your fictitious company at freenom domain register. we would use [dnsexit](https://dnsexit.com/domains/free-second-level-domains/)
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/003eb634-02c5-4d50-bada-220093c5a057" />

3. Create a hosted zone in AWS, and map it to your free domain from DNSexit
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/2f57eb22-088c-48d3-bf77-0a797893db29" />
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/cf5e743e-32d0-4856-88f9-c1fbee39858b" />


# Set Up a Virtual Private Network (VPC)
Always make reference to the architectural diagram and ensure that your configuration is aligned with it
1. **Create a VPC**
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/967e19ac-8b12-4ad5-a257-b600db270691" />

Enable DNS hostnames
Actions > Edit VPC Settings > Enable DNS hostnames
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/53306bd1-28c4-420b-8b6c-11f17b24540c" />

2. **Create subnets as shown in the architecture**. Thus we will create two public subnet in Availability zone A and B, respectively. futhermore, we will create four (4) private subnet in respect to the diagram we are working with. we will create two private subnet each in avaiability A and B.
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/69436f3c-e854-42d6-8a46-1bbf78029d63" />
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/46ba9bcc-eef4-4495-9320-9baedef70877" />
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/b8d7f852-f4c2-4e10-9750-b45ca72b0f60" />
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/29ec665d-4e7d-4f4f-a581-b38202ff88eb" />
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/793e081f-a362-4c5a-ae81-e9c182c7f821" />

Enable Auto-assign public IPv4 address in the public subnets. Actions > Edit subnet settings > Enable auto-assign public IPv4
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/6cf6ea66-a59d-4ae8-b4b1-064f5e771195" />

**All subnets**
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/bbdc7e52-b925-493c-8e2c-7a644fb16ba6" />

3. **Create a route table and associate it with public subnets**
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/230b82aa-3030-4a83-b611-39b7ddcdda43" />
Associate it with public subnets. Click on the route table > Subnet Association > Edit Subnet Association.
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/83fefcad-5a3f-4e63-8117-3158407e6cbd" />

The public route table and its associated public subnets
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/7726d9de-7bae-44b7-8189-df1273ad7641" />


4. **Create a route table and associate it with private subnets**
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/06681418-8886-469f-b9be-427d254ecb13" />

Associate it with private subnets. Click on the route table > Subnet Association > Edit Subnet Association.
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/32dfc4c6-88c6-49a0-9299-2ec6bc7a01b8" />

The private route table and its associated private subnets
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/b8cb50ff-3864-4d64-853f-68141fc77ecc" />

5. **Create an Internet gateway**
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/47a36245-3254-424e-af44-b7c348b923d3" />
Attach internet gateway to VPC.. Click on Actions > attach VPC,and select your vpc and attach internet gateway to VPC
<img width="950" height="314" alt="image" src="https://github.com/user-attachments/assets/078ae61b-8cb3-4794-8431-2712d08d2f7d" />

6. **Edit a route in public route table, and associate it with the internet gateway. (This is what allows a public subnet to be accessible from the internet)**
In the public route table, Click route tab > Edit route > Add route since we are routing traffic to the internet, the destination will be 0.0.0.0/0
<img width="950" height="315" alt="image" src="https://github.com/user-attachments/assets/9b367071-0385-43cb-8326-4d1b3ffce131" />
<img width="950" height="428" alt="image" src="https://github.com/user-attachments/assets/81cf4c43-93cf-47ec-a347-41a5782d7d66" />

7. **Create 3 Elastic IPs**
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/2d05d9d6-8b73-4f3d-95b6-5a7d548ea117" />
<img width="949" height="223" alt="image" src="https://github.com/user-attachments/assets/4afa8691-4a5f-4ac4-b12d-9f7bae1b3caf" />

8. **Create a NAT Gateway and assign one of the Elastic IPs(The other two will be used by Bastion hosts)**
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/8dd0679d-1609-4811-86b2-c44944e33e1e" />
<img width="950" height="362" alt="image" src="https://github.com/user-attachments/assets/775f5c70-2469-43c4-b6d4-0bb973f8994a" />

- Edit a route in private route table, and associate it with the Nat Gateway.
<img width="950" height="290" alt="image" src="https://github.com/user-attachments/assets/4dc3cce0-d6f1-420b-a823-6a5f7de394da" />

9. Create a Security Group for:
- **Nginx Servers:** Access to Nginx should only be allowed from a [Application Load balancer (ALB)](https://aws.amazon.com/elasticloadbalancing/application-load-balancer/)
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/d74bf1b9-c4d9-489b-874b-bed94ceab2b6" />

- **Bastion Servers:** Access to the Bastion servers should be allowed only from workstations that need to SSH into the bastion servers. Hence, you can use your workstation public IP address. To get this information, simply go to your terminal and type curl www.canhazip.com
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/3dd3b479-1140-46c0-b997-b2b788b859dc" />

- **Application Load balancer:** ALB will be available from the internet
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/e7ab52c9-20cf-4688-9b68-1bc91a93e2b2" />

- **Internal Application Load Balancer:** This is not an internet facing ALB rather used to distribute internal traffic comming from Nginx (reverse proxy) to our Webservers Auto Scalling Groups in our private subnets. It also helps us to prevent single point of failure.
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/aeb135af-1317-4696-9cbc-c1cc762de6f5" />


- **Webservers:** Access to webservers should only be allowed from the internal ALB.
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/519a1d1e-3ffd-46c7-984d-c1eccaf5b82c" />

- **Data Layer:** Access to the Data layer, which is comprised of Amazon Relational Database Service (RDS) and Amazon Elastic File System (EFS) must be carefully desinged - only webservers should be able to connect to RDS, while Nginx and Webservers will have access to EFS Mountpoint.
<img width="950" height="505" alt="image" src="https://github.com/user-attachments/assets/c7f638e4-d3ca-49c8-b252-e19eb5308c3c" />

**All security group**
<img width="950" height="273" alt="image" src="https://github.com/user-attachments/assets/351e472b-f53a-4ad3-a130-69888ed64157" />




