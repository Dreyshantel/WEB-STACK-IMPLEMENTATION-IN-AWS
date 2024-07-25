# DevOps Tooling Website Solution
### Overview
This project introduce a set of DevOps tools that will help our team in day to day activities in managing, developing, testing, deploying and monitoring different projects.

### Setup and Technologies
In this project, a tooling website solution which makes access to DevOps tools within the corporate infrastructure easily accessible will be implemented. These consist of following components:
1. **Infrastructure:** AWS
2. **Webserver Linux:** Red Hat Enterprise Linux 8
3. **Storage Server:** Red Hat Enterprise Linux 8 + NFS Server
4. **Database server:** Ubuntu 24.04 + MySQL
5. **Programming language:** PHP
6. **Code Repository:** GitHub


### Step 1- Prepare NFS Server
Spin up a new EC2 instance with RHEL Linux 8 Operating system
![Screenshot (510)](https://github.com/user-attachments/assets/20ba2a24-cee9-488e-a6a8-6ff075299f03)

Configure the inbound rules to allow traffic from SSH, HTTP, and HTTPS traffic from the internet
![Screenshot (511)](https://github.com/user-attachments/assets/57547066-17f9-4f47-908f-7c7dee7be571)

Configure storage and add 3 volumes to the EC2 instance
![Screenshot (512)](https://github.com/user-attachments/assets/bea72110-2013-4125-94f6-5e561482f8bb)

Use "lsblk" command to inspect what storage block is attached to the server
![Screenshot (513)](https://github.com/user-attachments/assets/5e1290e6-5d53-4483-82e7-ffce09611be8)

Use gdisk to create a single partition on each of the 3 disk
![Screenshot (514)](https://github.com/user-attachments/assets/e9acb228-dbbc-401a-a158-2b21a59264ae)

use "lsblk" command to confirm creation of partition of the 3 disk
![image](https://github.com/user-attachments/assets/313c2d50-92da-4a15-b41a-7f7fc9cf0d1d)

Update package repository and install lvm2 on the server.
![Screenshot (516)](https://github.com/user-attachments/assets/026cd05e-d00c-43f8-aaa5-183c3a8a9946)

Use `lvmdiskscan` to check available partitions
![image](https://github.com/user-attachments/assets/39fcbf2a-740f-4541-accd-a4e137d0c496)

Use the lvcreate utility to create logical volumes
```
sudo lvcreate -n lv-apps -L NFS-vg
sudo lvcreate -n lv-logs -L NFS-vg
sudo lvcreate -n lv-opt -L NFS-vg
```
![image](https://github.com/user-attachments/assets/bcc6fd6a-901e-4dac-a3f0-c17caa1a3492)

Use mkfs.xfs to format the logical volumes with xfs filesystem
```
sudo mkfs -t xfs dev/NFS-vg/lv-apps
sudo mkfs -t xfs /dev/NFS-vg/lv-logs
sudo mkfs -t xfs /dev/NFS-vg/lv-opt
```
![Screenshot (519)](https://github.com/user-attachments/assets/4b81749b-2136-478a-abf3-57529cd88b89)



