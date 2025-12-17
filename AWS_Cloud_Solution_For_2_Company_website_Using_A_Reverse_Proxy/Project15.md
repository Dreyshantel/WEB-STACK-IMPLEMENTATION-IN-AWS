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






  


