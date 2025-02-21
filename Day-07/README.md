# VPC with public-private subnet in production 
## Services used 
### -VPC, EC2, Automatic Scaling Group, Launch Templates, Security Group, Linux, Load Balancer.

## Description

####  - This project demonstrates how to create a VPC that can be used for server in a production environment 
####  - To improve resiliency, Deployed the server in two availability Zones, by using an Auto Scaling group and an Application Load Balancer. 
#### - For additional security, deployed the server in private subnet. 
####  - The servers receive request through the load balancer. The server can connect o the internet by using a NAT gateway. To improve resiliency, deployed the gateway in both Availability Zones.

## Overview

#### - The VPC has public subnets and private subnets in two Availability Zones.
#### - Each public subnet contains a NAT gateway and a load balancer node.
#### - The servers run in the private subnets, are launched and terminated by using an Auto Scaling group, and receive traffic from the Load Balancer.
#### - The servers can connect to the internet by using the NAT gateway. 



![1](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/a3b48941-a7d4-4536-927b-e53e9be642cf)
## Lets start with creating a VPC (Virtual Private Cloud).
### Required Configuration for VPC 
-	Select option - VPC and more 
-	Name the VPC 
-	Number of Availability Zones - 2 (for High Availability)
-	Number of Public Subnet - 2
-	Number of Private Subnet - 2
-	Number of NAT Gate - 1 per AZ (For masking of application IP addresses)
-	VPC  endpoints  -  None (As Database is not required – 2 Tier Application)

![Screenshot (248)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/f57da3d5-39ad-444c-82ab-d2a1bbbb4892)
![Screenshot (251)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/0a143e7c-832a-4205-a2e2-fa5a0a6730df)
![Screenshot (252)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/e44671a8-7eab-4903-a453-a8ceeb895d8d)

## Hit Create VPC And wait for this Outcome 

![Screenshot (253)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/cb2fecd0-859d-40b2-bbf6-127de740abe8)
![Screenshot (254)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/92ddedb9-e2be-4647-860e-d55c0d4f09a7)

## Now we will Create Auto Scaling Group and for creating Auto Scaling Group we need to create Launch template as Launch template is used as reference for creation of instances in future during downtime 
-	Search for EC2 
-	Search for Auto Scaling Group
-	Click on Create Launch Template

![Screenshot (255)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/bb15e395-33b7-4256-993b-12d1c0e14281)

-	Name the Template
-	Provide Description

![Screenshot (256)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/37b8c231-6103-4cbb-b653-55496ea542c4)

-	Choose Operating System as Ubuntu (For Linux based applications)
  
![Screenshot (257)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/bb5190e8-1c3f-4f2d-8e63-dbeb114967c7)

-	Choose Instance Type (As per Requirements) I am using t2 micro (free tier applicable)
-	Key Pair (For Login Authentication)

![Screenshot (258)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/1959d61e-91f6-4d8e-b9b0-b1128334129e)

-	Click Network Settings (For Creating Security Group with Inbound Rules)
       -   Select create security group
       -   Name the security group
       -   Provide description
       -   select VPC ( select VPC just created  )
       -   security rule 1 = Type ssh, Port 22, Source Type  anywhere  (for SSH login)
       -   security rule 2 = Type Custom TCP, Port 8000, Source Type  anywhere  
- Hit Create
![Screenshot (259)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/3df33470-cbf1-486e-b4bf-519016473fe4)
![Screenshot (260)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/c2063573-e280-4555-9321-0f96ca46e2c8)

### After creation of Launch Template we will create Auto Scaling Group
Step 1 -
-	Name the auto scaling group 
-	Select the launch template we have just created
-	Hit next 


![Screenshot (261)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/a35cfd72-487d-4977-a306-c1476b37fd2e)
![Screenshot (262)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/97779458-7ad8-4e6a-a347-0110ac2194bd)

Step 2 – 
-	Choose VPC last created

![Screenshot (263)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/66f1d8ce-8cd4-4e1e-b2b8-2add32e01372)

-	Choose the private subnet of both the AZ ( As application needs to be in private )
-	Hit next

![Screenshot (264)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/7b075ca4-7e83-4a8d-85ac-4eaca3751083)

Step 3 – 
-	I have not attached any load balancer at Auto Scaling Group private subnet for applications
-	Health check is Okay
-	Hit next
  
Step 4
-	Desired capacity – 2
-	Minimum capacity – 1
-	Maximum capacity – 4 (if traffic increases for applications it can expand up to 4 machines)
-	Scaling policies – None 
-	Hit next

![Screenshot (265)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/e7dc45ba-190c-4bc7-bb52-3adcdcfb43d8)
![Screenshot (266)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/202310b6-7968-4e64-a563-98def333da31)

Step 5 
-	Here we can use SNS for notification about machine if added or terminated (I have not used SNS in this project)
-	Hit next 

![Screenshot (267)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/c7774005-f678-4479-bb30-7e8221e6e376)

Step 6 
-	Here we can use tags for using this Auto Scaling Group in future (I am using tags in this project )
-	Hit next
  
Step 7 
-	Review all the configurations of tha Auto Scaling Group 
-	Hit Create Auto Scaling Group 


![Screenshot (268)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/d3f5cd1c-da8a-4c4e-ab09-ca84e46c474e)

## Now  check EC2 instance if Auto Scaling Group have created 2  instances in the selected subnet at step 2 of Auto Scaling Group Creation
After checking the subnets, we will install the applications  in both the private instances 
## Bastion Host
For installing applications, we need to connect the private application server. To connect the private server, we need to create Bastion Host ( As public IP is not present to SSH the private application server ) 
For creating Bastion Host, we will Create an EC2 instance 
-	Name instance Bastion Host
-	Select the image ubuntu (For Linux based applications )

![Screenshot (269)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/b4acb153-4524-4e1d-a23a-4eedb3fb0274)

-	Instance type – t2 micro
-	Select Key pair to ssh connect the Bastion Host

![Screenshot (270)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/8dd31bd7-1656-40c0-a894-20bb15b27f67)

-	Network setting - select the same VPC as created above 
-	Add security group which allow SSH connect to Bastion Host 
-	Enable assign public IP
-	Hit Launch Instance 

![Screenshot (272)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/1f6b847f-3d4e-4f70-9964-28173af3915c)

## Copy the key pair (pem file) from local computer to the bastion server using the SCP command
-	scp -i /path/key-pair-name.pem /home/ubuntu/key.pem ubuntu@IP ADDRESH:/home/ubuntu

![Screenshot (273)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/9bf0a21a-ed7e-4f4c-9f58-a679af558731)

## Connect to the Bastion Host through ubuntu terminal using SSH command 
-	ssh -i /path/key-pair-name.pem instance-user-name@instance-public-ip-address

After establishing the connection with Bastion Host, we will connect to the private application server  
Check if the pem file is present or not by using ( ls )command

![Screenshot (275)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/6520dbf3-0f89-415a-8c3f-a1a22581921f)

## Connect to the private application server  through Bastion Host using SSH command 
-	ssh -i /path/key-pair-name.pem instance-user-name@instance-private-ip-address

![Screenshot (276)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/1003de8f-89db-4391-834b-362c548530d5)

 ## Install the application 
-	Make a simple vim file  ( vim index.html )and  write this simple html code 
<! DOCTYPE html> 

 
 #### My AWS PROJECT to demonstrate apps in private subnet 



-	then run the python command at port 8000

- python3 -m http.server 8000

![Screenshot (277)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/5f893d83-fa23-4dc9-9a3a-3b52767bbcb5)

## Create Application Load Balancer which works with HTTP & HTTTPS - ( Level 7 Load Balancer )
- Select application load balancer

![Screenshot (278)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/22a9b952-8df0-46bc-be94-870fc29bc12f)

- Name the load balancer 
-  Select – Internet facing 
-  Select IPV4

![Screenshot (279)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/09c8e48c-ccf7-4641-8c31-301d50d7657e)

- Select VPC which is created above 
- Select both the Availability Zones only with public subnet
- Select Security Group with Port 80 from anywhere

![Screenshot (281)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/80fc9291-6466-405b-bf94-f6274200ca78)

## For Load Balancer - Create target Group 
-	Select instance 

![Screenshot (283)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/d62f20ab-4dfd-44b7-a5a6-3cee73f478e2)

-	Name the target group 

![Screenshot (284)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/f2333213-06ba-408e-bdc3-e6bcf205fa1b)

-	Select HTTP port 8000
-	Hit next
  
![Screenshot (285)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/e9a71939-454c-49a4-b742-fe04081e854e)

### Select the instance as target for load balancer that were created above by Auto Scaling Group

![Screenshot (287)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/d6c8cea0-baa4-4f12-9ccd-b6c2f121c14c)

Port for selected instance – 8000
Click – Include as pending below
-	Hit Create Target Group 

![Screenshot (288)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/5cda846b-802d-4bc5-a99b-2a5e9a331c5d)

### After creation of the target group 
Add this target group to the Load Balancer 
-	Select protocol – HTTP 
-	Open port – 80
-	Select Default action – VPC created above 

![Screenshot (289)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/f3327e7d-7ca2-4526-8b33-e418619de56d)

Hit create Load Balancer 

![Screenshot (290)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/2e5f3992-e906-4629-9bd2-aa8e4e7016f1)

Now Go to the Load Balancer – If its showing error for port 80 then change the security group and allow HTTP ,TCP,80,Anywhere

![Screenshot (291)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/9727d8dc-e514-422c-a561-370f92025fcc)
![Screenshot (292)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/17cf24c4-483d-4fe8-a794-594034d9f382)

## Now Copy the DNS name link and paste on your Browser to Look your HTML page
![Screenshot (293)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/1953a5ce-1776-4cb2-ad8f-cca7233d404c)

## My AWS PROJECT to demonstrate apps in private subnet 

![Screenshot (294)](https://github.com/TheMannu/project-AWS-VPC-with-public-private-subnet-/assets/84488161/ba52f72a-0380-4cbb-bd3d-c7faf6d1d11c)
