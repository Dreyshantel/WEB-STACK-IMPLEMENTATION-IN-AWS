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

# TLS Certificates From Amazon Certificate Manager (ACM)
We will need TLS certificates to handle secured connectivity to our Application Load Balancers (ALB).
1. Navigate to AWS ACM
2. Request a public wildcard certificate for the domain name we registered in Cloudns
3. Use DNS to validate the domain name
4. Tag the resource
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/69f4445a-90e5-4fdb-999d-3cd6dbbc2f37" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/9f32af7d-e261-445b-8c80-e4f98975a1be" />

Ensure to create record on Route 53 after creating the certificate. This will generate a CNAME record type in Route 53. We will attach this certificate to all the load balancers.

- Click, Create records in Route 53 > Create records
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/1065a878-0d0c-41e0-b983-28c19b4354aa" />

Copy the CNAME name and the CNAME value generated then Go to dnsexit, Click on CNAME > Add new record. Paste the CNAME name and value to validate the record for AWS to issue the certificate (The Certificate status remain pending unitl the record is validated by the Domain provider.)
<img width="960" height="198" alt="image" src="https://github.com/user-attachments/assets/c0e7339e-84e2-4632-bdcc-e66e4d97a3c2" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/533e19c7-0785-423a-b450-29c8f6481a7f" />

# Setup EFS
Amazon Elastic File System (Amazon EFS) provides a simple, scalable, fully managed elastic Network File System (NFS) for use with AWS Cloud services and on-premises resources. In this project, we will utulize EFS service and mount filesystems on both Nginx and Webservers to store data.

1. Create an EFS filesystem
2. Create an EFS mount target per AZ in the VPC, associate it with both subnets dedicated for data layer. NB: Any subnet we specify our mount target, the Amazon EFS becomes available in that subnet. As such, we will specify it in private webserver subnets so that all the resources in that subnet will have the ability to mount to the file system.
3. Associate the Security groups created earlier for data layer.
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/592b1b9d-f104-419d-a22c-5e7864504ddf" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/4c4edb38-f049-4fc6-aa46-70b89d1f6afc" />
<img width="960" height="250" alt="image" src="https://github.com/user-attachments/assets/c6d43165-2752-4437-9c1e-4fe3d09618c3" />

4. Create an EFS access point.
This will specify where the webservers will mount with, thus creating 2 mount points for Tooling and Wordpress servers each.

Access point for wordpress server
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/576fb71d-1868-423e-b170-ec848174028c" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/8db24d2e-667f-47e8-ae6e-9eac023cb75b" />
Access point for tooling server
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/1cad061a-63ab-440a-a441-c20a76ff6675" />
<img width="960" height="408" alt="image" src="https://github.com/user-attachments/assets/c7548433-744f-451f-a82a-b1684f985439" />

EFS Access points
<img width="960" height="170" alt="image" src="https://github.com/user-attachments/assets/b0acaaf3-0af7-43c6-b59d-c7b708ff5981" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/83ef2d09-d24b-4749-863a-e2ae78e73e0d" />

# Setup RDS





# Proceed with Compute resources
you will need to set up and configure computer resources inside your VPC. The resources related to compute are:

- EC2 Instances
- Launch Template
- Target Groups
- Autoscaling Groups
- TLS Certificates
- Application Load Balancers (ALB)

# Set Up Compute Resources for Nginx, Bastion and Webservers
NB: To create the Autoscaling Groups, we need Launch Templates and Load Balancers. The Launch Templates requires AMI and Userdata while the Load balancer requires Target Group
## Provision EC2 Instances for Nginx, Bastion and Webservers
1. Create EC2 Instances based on CentOS Amazon Machine Image (AMI) per each Availability Zone in the same Region. Use EC2 instance of T2 family (e.g. t2.micro or similar) with security group (All traffic - anywhere).


2. Ensure that it has the following software installed:
- python
- ntp
- net-tools
- vim
- wget
- telnet
- epel-release
- htop

## For Nginx, Bastion and Webserver.
**For Nginx**
```
sudo yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
```
