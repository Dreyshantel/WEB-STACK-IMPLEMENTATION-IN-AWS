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
## Pre-requisite:
Create a KMS key from Key Management Service (KMS) to be used to encrypt the database instance.
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/c326eb29-80a5-49b0-98b0-fa471c061921" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/f24e55c6-16fe-4a34-9afb-36118a31906a" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/43572ec6-1931-4cca-9fdc-849b97f0dfac" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/bbe99cf2-829f-4274-8421-bfcd74b63ae7" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/c8130072-aa1f-491b-a300-b40165eeacb6" />

Amazon Relational Database Service (Amazon RDS) is a managed distributed relational database service by Amazon Web Services. This web service running in the cloud designed to simplify setup, operations, maintenances & scaling of relational databases. Without RDS, Database Administrators (DBA) have more work to do, due to RDS, some DBAs have become jobless. To ensure that your databases are highly available and also have failover support in case one availability zone fails, we will configure a multi-AZ set up of RDS MySQL database instance. In our case, since we are only using 2 AZs, we can only failover to one, but the same concept applies to 3 Availability Zones. We will not consider possible failure of the whole Region, but for this AWS also has a solution - this is a more advanced concept that will be discussed in following projects.

To configure RDS, follow steps below:
1. Create a subnet group and add 2 private subnets (data layer)
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/b6308e03-466d-49f5-9c21-248aa44299a7" />
<img width="960" height="186" alt="image" src="https://github.com/user-attachments/assets/6c1031f0-b7f4-4eaa-a1b2-08f4f5be3b02" />

2. Create an RDS instance for mysql 8.*.*
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/9168d3a4-7423-46dc-a6ac-97f357b86bfd" />

3. To satisfy our architectural diagram, you will need to select either Dev/Test or Production Sample Template. But to minimize AWS cost, you can select the Do not create a standby instance option under Availability & durability sample template (The production template will enable Multi-AZ deployment)
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/1e1d3dbc-52f1-4b4a-a939-18c71e9f317a" />

4. Configure other settings accordingly (For test purposes, most of the default settings are good to go). In the real world, you will need to size the database appropriately. You will need to get some information about the usage. If it is a highly transactional database that grows at 10GB weekly, you must bear that in mind while configuring the initial storage allocation, storage autoscaling, and maximum storage threshold.
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/44221a3e-9293-46ba-864d-045436c1682d" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/802eb36c-0bfb-42c7-91e3-ada4fc0eddac" />

5. Configure VPC and security(ensure the database is not available from the internet)
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/53999c26-5b0e-4bbc-bb26-e3845472b2ed" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/24f4cbe6-ff04-4122-b85c-410c3f2af9bd" />

6. Configure backups and retention
7. Encrypt the database using the KMS key created earlier
8. Enable Cloudwatch monitoring and export Error and Slow Query logs (for production, also include Audit)
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/0ea6c2c7-a366-497c-99af-8a95e9d20f1c" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/ea350657-f3ad-4416-9286-5f5df1afdc3e" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/1c43d2fa-1125-4e73-8751-74b11a597765" />
<img width="960" height="230" alt="image" src="https://github.com/user-attachments/assets/a9781367-1a0b-46cd-91c8-3331bc7064bb" />




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
1. Create EC2 Instances based on CentOS Amazon Machine Image (AMI) per each Availability Zone in the same Region. Use EC2 instance of T2 family (e.g. t2.micro or similar) with security group (All traffic - anywhere). We will use our custom VPC with public subnets hosting the Bastion and Nginx servers, while application web servers were deployed in private subnets and accessed through a load balancer


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
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/1d8102db-0e3c-4e4a-97ea-5161fb05631e" />

```
sudo yum install -y dnf-utils http://rpms.remirepo.net/enterprise/remi-release-9.rpm

sudo yum install wget vim python3 telnet htop git mysql net-tools chrony -y

sudo systemctl start chronyd
sudo systemctl enable chronyd
sudo systemctl status chronyd
```
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/e04dcef2-10fe-405f-b204-72181ea75e03" />

NB: Repeat the above steps for Bastion and Webservers

#Nginx and Webserver (Set SELinux policies so that our servers can function properly on all the redhat instance).
For Nginx
```
sudo setsebool -P httpd_can_network_connect=1
sudo setsebool -P httpd_can_network_connect_db=1
sudo setsebool -P httpd_execmem=1
sudo setsebool -P httpd_use_nfs 1
```
<img width="928" height="82" alt="image" src="https://github.com/user-attachments/assets/031a6094-ae6f-46e3-a6d4-5576dd809282" />

NB: Repeat the step above for Webservers

# Install Amazon EFS utils for mounting the targets on the Elastic file system (Nginx and Webserver).
For Nginx
```
git clone https://github.com/aws/efs-utils

cd efs-utils
```
<img width="905" height="193" alt="image" src="https://github.com/user-attachments/assets/f62e92b2-940e-460a-a6fa-a23513a17661" />


```
sudo yum install -y make
```
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/812244b5-7506-4823-b72b-3c3bf12d57f5" />


```
sudo yum install -y rpm-build
```
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/a690b648-db59-4d32-8e40-274f0dedcbfe" />


```
# openssl-devel is needed by amazon-efs-utils-2.0.4-1.el9.x86_64
sudo yum install openssl-devel -y
```
<img width="957" height="391" alt="image" src="https://github.com/user-attachments/assets/7eb1929f-9639-42ae-8836-bd605fcf0ee6" />

```
# Cargo command needs to be installed as it is necessary for building the Rust project included in the source.
sudo yum install cargo -y
```
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/a4e57107-bcf0-4543-953a-a99bb8b8a200" />


```
sudo make rpm
```

```
sudo yum install -y  ./build/amazon-efs-utils*rpm
```
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/1143a9d2-3722-45c9-94c4-0b27fc9e1302" />

**Repeat the steps above for Webservers**
We are using an ACM (Amazon Certificate Manager) certificate for both our external and internal load balancers. But for some reasons, we might want to add a self-signed certificate:

- Compliance Requirements:
  - Certain industry regulations and standards (e.g., PCI DSS, HIPAA) require end-to-end encryption, including between internal load balancers and backend servers (within a private network).
    
- Defense in Depth:
  - Adding another layer of security by encrypting traffic between internal load balancers and web servers can provide additional protection.
 
When generating the certificate, In the common name, enter the private IPv4 dns of the instance (for Webserver and Nginx). We use the certificate by specifying the path to the file devcloud.crt and devcloud.key in the nginx configuration.

# Set up self-signed certificate for the nginx instance
```
sudo mkdir /etc/ssl/private

sudo chmod 700 /etc/ssl/private

sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/devcloud.key -out /etc/ssl/certs/devcloud.crt
```
<img width="953" height="268" alt="image" src="https://github.com/user-attachments/assets/2d1bc4ff-10b2-4ac0-86bd-9d3d73056647" />

```
sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048
```
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/cf6e1897-3986-4d2e-906c-a3bed876f43c" />


# Set up self-signed certificate for the Apache Webserver instance
```
sudo yum install -y mod_ssl

sudo openssl req -newkey rsa:2048 -nodes -keyout /etc/pki/tls/private/devcloud.key -x509 -days 365 -out /etc/pki/tls/certs/devcloud.crt
```
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/f535280c-4609-4559-a6f1-0562976b7e0b" />


```
# Edit the ssl.conf to conform with the key and crt file created.
sudo vim /etc/httpd/conf.d/ssl.conf
```
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/ac94fa6d-3447-476b-ba65-a8cadf86702c" />

# Create an AMI out of the EC2 instances
On the EC2 instance page, Go to Actions > Image and templates > Create image
**For Bastion AMI**
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/d0731a56-8b8c-4399-accc-14db11448e18" />

**For Nginx AMI**
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/f61ccd32-d4da-4855-84a7-75c26c31f09d" />

**For webserver AMI**
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/5a95502d-6b82-4bc8-af9d-032f5da87e4c" />

All AMIs
<img width="956" height="170" alt="image" src="https://github.com/user-attachments/assets/84f36668-0cb2-436f-ba46-0f6b5a11d2b9" />

# CONFIGURE TARGET GROUPS
Create Target groups for Nginx, Worpress and Tooling

**For Nginx Target Group**
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/19724820-464b-4c10-a8d6-54397bbdcba2" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/31b7e146-dcfb-4c25-bd9d-7f7877d087fc" />


**For Wordpress Target Group**
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/8ab56245-f32c-46cf-9141-babb76a9b1d8" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/1c015c8e-f6e4-4ba1-9bc7-8af02ec1f72b" />

**For Tooling Target Group**
<img width="251" height="108" alt="image" src="https://github.com/user-attachments/assets/7771d5af-ead3-4e05-920b-0f868e742469" />

# Configure Application Load Balancer (ALB)
**External Application Load Balancer To Route Traffic To NGINX**
Nginx EC2 Instances will have configurations that accepts incoming traffic only from Load Balancers. No request should go directly to Nginx servers. With this kind of setup, we will benefit from intelligent routing of requests from the ALB to Nginx servers across the 2 Availability Zones. We will also be able to offload SSL/TLS certificates on the ALB instead of Nginx. Therefore, Nginx will be able to perform faster since it will not require extra compute resources to validate certificates for every request.

1. Create an Internet facing ALB
2. Ensure that it listens on HTTPS protocol (TCP port 443)
3. Ensure the ALB is created within the appropriate VPC | AZ | Subnets
4. Choose the Certificate from ACM
5. Select Security Group
6. Select Nginx Instances as the target group

<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/375942c5-e5da-469d-b360-7e7b48a9f332" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/e42ed378-a2df-4dff-8e2d-3130301cddea" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/aa53bd3f-fa65-44bd-ab1c-79b5cfac0e00" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/d52cfafc-ac68-443b-bae7-d2e748964608" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/242f375b-bc28-472d-b1ae-56b7246dca63" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/960d3415-6246-4522-bfd0-df74c4cd6591" />


# Application Load Balancer To Route Traffic To Webservers
Since the webservers are configured for auto-scaling, there is going to be a problem if servers get dynamically scalled out or in. Nginx will not know about the new IP addresses, or the ones that get removed. Hence, Nginx will not know where to direct the traffic.

To solve this problem, we must use a load balancer. But this time, it will be an internal load balancer. Not Internet facing since the webservers are within a private subnet, and we do not want direct access to them.
1. Create an Internal ALB
2. Ensure that it listens on HTTPS protocol (TCP port 443)
3. Ensure the ALB is created within the appropriate VPC | AZ | Subnets
4. Choose the Certificate from ACM
5. Select Security Group
6. Select webserver Instances as the target group
7. Ensure that health check passes for the target group

**NOTE:** This process must be repeated for both WordPress and Tooling websites.
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/9e8e5410-9614-4ae9-b791-b44b1bec7e74" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/93172f1d-2e08-4def-ae95-8ab6c9767775" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/8af428f5-e910-4167-921f-7d0a5a44741a" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/8029e738-8e4f-407b-a31b-b3d905ee8e54" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/befbefde-c4d1-41e9-aa2a-9efc2b77393b" />
<img width="820" height="161" alt="image" src="https://github.com/user-attachments/assets/b10fc920-a8d9-4fcf-81ed-ca6a97101e7e" />


# PREPARE LAUNCH TEMPLATE FOR NGINX (ONE PER SUBNET)
1. Make use of the AMI to set up a launch template
2. Ensure the Instances are launched into a public subnet.
3. Assign appropriate security group.
4. Configure Userdata to update yum package repository and install nginx. Ensure to enable auto-assign public IP in the Advance Network Configuration


We need to update the reverse.conf file by updating proxy_pass value to the end point of the internal load balancer (DNS name) before using the userdata so as to clone the updated repository.
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/17c5c341-1470-465e-bf31-872917a46e5f" />

Repeat the same setting for Bastion, the difference here is the userdata input, AMI and security group.

# Wordpress Userdata
NB: Both Wordpress and Tooling make use of Webserver AMI.

Update the mount point to the file system, this should be done on access points for tooling and wordpress respectively.

sudo mount -t efs -o tls,accesspoint=fsap-0941d279e4caf2238 fs-01bb3fe22fdd61691:/ /var/www/


The RDS end point is also needed. Paste the rds end-point in the wordpress userdata and tooling userdata
```
#!/bin/bash

exec > /var/log/tooling.log 2>&1

# Create directory and mount EFS
mkdir -p /var/www/
sudo mount -t efs -o tls,accesspoint=fsap-0959937723766fece fs-01db9201ff82862d8:/ /var/www/


# Install and start Apache
sudo yum install -y httpd
    types_hash_max_size 2048;
sudo systemctl start httpd
sudo systemctl enable httpd

# Install PHP and necessary extensions
sudo yum module reset php -y
sudo yum module enable php:remi-7.4 -y
sudo yum install -y php php-common php-mbstring php-opcache php-intl php-xml php-gd php-curl php-mysqlnd php-fpm php-json
sudo systemctl start php-fpm
sudo systemctl enable php-fpm

# Clone the tooling repository
git clone https://github.com/Dreyshantel/tooling.git

mkdir -p /var/www/html/
cp -R tooling/html/* /var/www/html/

# Setup MySQL database
cd /tooling
mysql -h devcloud-database.c3w0g802qw96.eu-north-1.rds.amazonaws.com -u adminuser -p toolingdb < tooling-db.sql


cd /var/www/html/
touch healthstatus

# Update database connection in PHP functions file
sed -i "s/$db = mysqli_connect('mysql.tooling.svc.cluster.local', 'admin', 'admin', 'tooling');/$db = mysqli_connect('devcloud-database.c3w0g802qw96.eu-north-1.rds.amazonaws.com', 'adminuser', '$Devops123!', 'testdb');/g" functions.php


# Fix SELinux permissions
chcon -t httpd_sys_rw_content_t /var/www/html/ -R

# Disable Apache welcome page and restart Apache
sudo mv /etc/httpd/conf.d/welcome.conf /etc/httpd/conf.d/welcome.conf_backup
sudo systemctl restart httpd
```

All launch templates created.
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/9298ca16-2c60-4faf-940a-20b6b138fbba" />


# CONFIGURE AUTOSCALING FOR NGINX
1. Select the right launch template
2. Select the VPC
3. Select both public subnets
4. Enable Application Load Balancer for the AutoScalingGroup (ASG)
5. Select the target group you created before
6. Ensure that you have health checks for both EC2 and ALB
7. The desired capacity is 2
8. Minimum capacity is 2
9. Maximum capacity is 4
10. Set scale out if CPU utilization reaches 90%
11. Ensure there is an SNS topic to send scaling notifications

## Create Auto Scaling Group for Bastion
   <img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/7a42fc51-d18b-4837-a9fe-ae0606a63198" />

**Access RDS through Bastion connection to create database for wordpress and tooling.**

Copy the RDS endpoint to be used as host
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/a10341eb-ce5a-47b3-8dd5-e9cfeaf8d10f" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/8af7c300-05dd-4f9b-b69e-942b2f0d804f" />
<img width="960" height="446" alt="image" src="https://github.com/user-attachments/assets/a13a487a-347c-473b-86dd-caa6002844fd" />

## Create Auto Scaling Group for Nginx and repeat the Nginx Auto Scaling Group steps above for Wordpress and Tooling with their right launch template
All Auto Scaling Groups
<img width="960" height="179" alt="image" src="https://github.com/user-attachments/assets/a4d5e1d7-7eb8-4c18-9f98-725825eefcd1" />


# Configuring DNS with Route53
Earlier in this project we registered a free domain with Cloudns and configured a hosted zone in Route53. But that is not all that needs to be done as far as DNS configuration is concerned.

We need to ensure that the main domain for the WordPress website can be reached, and the subdomain for Tooling website can also be reached using a browser.

Create other records such as CNAME, alias and A records.

NOTE: You can use either CNAME or alias records to achieve the same thing. But alias record has better functionality because it is a faster to resolve DNS record, and can coexist with other records on that name. Read here to get to know more about the differences.

- Create an alias record for the root domain and direct its traffic to the ALB DNS name.
- Create an alias record for tooling.devcloud-dynamic.linkpc.net and direct its traffic to the ALB DNS name.
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/89e8c4df-86f7-43e8-a124-71f65d0bb5cb" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/86fa65cf-6f15-4a31-8633-04a51b4d65e9" />
<img width="960" height="510" alt="image" src="https://github.com/user-attachments/assets/310852ad-820a-4028-8973-5fb4498f2eb2" />

## Let's access the website
<img width="1920" height="1020" alt="image" src="https://github.com/user-attachments/assets/077aa89f-3cd8-4440-b0c2-c8746157d473" />

