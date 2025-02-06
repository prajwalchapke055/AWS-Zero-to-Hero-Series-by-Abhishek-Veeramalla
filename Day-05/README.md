# AWS Security using Security Groups and NACL

AWS (Amazon Web Services) provides multiple layers of security to protect resources and data within its cloud infrastructure. Two important components for network security in AWS are Security Groups and Network Access Control Lists (NACLs). Let's explore how each of them works:

    Security Groups:
        Security Groups act as virtual firewalls for Amazon EC2 instances (virtual servers) at the instance level. They control inbound and outbound traffic by allowing or denying specific protocols, ports, and IP addresses.
        Each EC2 instance can be associated with one or more security groups, and each security group consists of inbound and outbound rules.
        Inbound rules determine the traffic that is allowed to reach the EC2 instance, whereas outbound rules control the traffic leaving the instance.
        Security Groups can be configured using IP addresses, CIDR blocks, security group IDs, or DNS names to specify the source or destination of the traffic.
        They operate at the instance level and evaluate the rules before allowing traffic to reach the instance.
        Security Groups are stateful, meaning that if an inbound rule allows traffic, the corresponding outbound traffic is automatically allowed, and vice versa.
        Changes made to security group rules take effect immediately.

![AWS Security Groups (SGs)](https://docs.aws.amazon.com/images/vpc/latest/userguide/images/security-group-overview.png)

    Network Access Control Lists (NACLs):
        NACLs are an additional layer of security that operates at the subnet level. They act as stateless traffic filters for inbound and outbound traffic at the subnet boundary.
        Unlike Security Groups, NACLs are associated with subnets, and each subnet can have only one NACL. However, multiple subnets can share the same NACL.
        NACLs consist of a numbered list of rules (numbered in ascending order) that are evaluated in order from lowest to highest.
        Each rule in the NACL includes a rule number, protocol, rule action (allow or deny), source or destination IP address range, port range, and ICMP (Internet Control Message Protocol) type.
        NACL rules can be configured to allow or deny specific types of traffic based on the defined criteria.
        They are stateless, which means that if an inbound rule allows traffic, the corresponding outbound traffic must be explicitly allowed using a separate outbound rule.
        Changes made to NACL rules may take some time to propagate to all the resources using the associated subnet.

![Network Access Control Lists (NACLs)](https://docs.aws.amazon.com/images/vpc/latest/userguide/images/network-acl.png)

---

# AWS VPC Setup, EC2 Instance Launch, and HTTP Server Configuration

This guide walks you through the process of creating a custom VPC in AWS, launching an EC2 instance, setting up a simple Python HTTP server, and testing the network and firewall configurations.

## 1. Create a Custom VPC in AWS Console

1. **Search for VPC in the AWS Console**
   - Open your AWS Management Console and search for **VPC** in the search bar.
2. **Create a New VPC**
   - Click on **Create VPC** and configure the following settings:
     - **Name tag**: Demo
     - **IPv4 CIDR block**: `10.0.0.0/16`
     - **IPv6 CIDR block**: No
     - **Tenancy**: Default
     - **Number of AZs**: 2
     - **Number of public subnets**: 2
     - **Number of private subnets**: 2
     - **NAT**: None
     - **VPC Endpoints**: Select **S3 Gateway**
     - **DNS Options**: Enable DNS hostnames and enable DNS resolution
   - Click **Create VPC** to finalize the creation of the VPC.

---

## 2. Launch an EC2 Instance

1. **Search for EC2 in the AWS Console**

   - Open your AWS Management Console and search for **EC2** in the search bar.

2. **Launch an EC2 Instance**
   - Click on **Launch Instance** and configure the following settings:
     - **Name**: demp-instance
     - **AMI**: Ubuntu 22.04
     - **Instance Type**: t2.micro
     - **Key Pair**: Select `abc.pem`
     - **Network Settings**:
       - **VPC**: Select **Demo-vpc**
       - **Subnet**: Select **Demo-subnet-public1-us-east-1a**
       - **Auto-assign Public IP**: Enable
3. **Configure Security Group**
   - Create a new security group or select an existing one. Ensure that it allows inbound traffic for the required ports.
4. **Configure Storage**

   - Set the storage size to **8 GiB**.

5. **Launch the Instance**
   - Once all the configurations are complete, click **Launch** to start the EC2 instance.

---

## 3. Connect to EC2 Instance and Start a Simple HTTP Server

1. **Access the Instance via SSH**

   - Open a terminal on your local machine (e.g., PowerShell, iTerm, MobaXterm, or PuTTY) and run the following command:

   ```bash
   ssh -i abc.pem ubuntu@<public-ip-of-your-running-instance>
   ```

   - Replace `<public-ip-of-your-running-instance>` with the public IP of the running EC2 instance.

2. **Update System and Install Python**

   - Once logged into the EC2 instance, update the system and install Python by running:

   ```bash
   sudo apt update
   python3
   ```

   - Press `Ctrl+C` to exit the Python shell.

3. **Start the HTTP Server**

   - Start a simple Python HTTP server by running:

   ```bash
   python3 -m http.server 8000
   ```

4. **Test the HTTP Server in the Browser**
   - Open a browser and navigate to `http://<public-ip>:8000`. You should see the HTTP server's default directory listing.

---

## 4. Modify Security Group to Allow HTTP Access

1. **Edit Inbound Rules for the Security Group**

   - Go to the **Security Groups** section in the AWS Console and find the security group associated with your EC2 instance.
   - Click **Edit inbound rules**.
   - Add a rule to allow traffic on port 8000:
     - **Type**: Custom TCP
     - **Port range**: 8000
     - **Source**: Anywhere IPv4
   - Click **Save rules**.

2. **Refresh the Browser**
   - After saving the inbound rules, refresh the browser to see the output from the HTTP server.

---

## 5. Modify Network ACLs to Control Traffic

1. **Search for VPC in AWS Console**

   - Search for **VPC** in the AWS Console again and go to the **Network ACLs** section.

2. **Edit Inbound Rules for the Network ACL**

   - Edit the inbound rules for the Network ACL associated with the VPC and set the following:

     - **Rule Number 100**:

       - **Type**: All traffic
       - **Port range**: All
       - **Action**: Allow

     - **Rule Number 200**:
       - **Type**: Custom TCP
       - **Port range**: 8000
       - **Action**: Deny

   - Click **Save changes**.

3. **Test the Impact of ACL Changes**
   - Refresh the browser again and notice how the output is impacted based on the newly configured Network ACLs.

---

## Conclusion

You have successfully created a custom VPC, launched an EC2 instance, configured a Python HTTP server, adjusted security group settings, and applied Network ACL rules in AWS. This setup is helpful for controlling access to your EC2 instances and testing different network security configurations.

---

![Screenshot 2023-06-29 at 12 14 32 AM](https://github.com/iam-veeramalla/aws-devops-zero-to-hero/assets/43399466/30bbc9e8-6502-438b-8adf-ece8b81edce9)

---
