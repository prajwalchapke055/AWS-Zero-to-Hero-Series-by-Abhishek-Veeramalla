# Introduction to EC2 :-

## What is EC2, and why is it important?

- Amazon Elastic Compute Cloud (Amazon EC2) is a web service that provides secure, resizable compute capacity in the cloud.
- Access reliable, scalable infrastructure on demand. Scale capacity within minutes with SLA commitment of 99.99% availability.
- Provide secure compute for your applications. Security is built into the foundation of Amazon EC2 with the AWS Nitro System.
- Optimize performance and cost with flexible options like AWS Graviton-based instances, Amazon EC2 Spot instances, and AWS Savings Plans.

---

## EC2 usecases

- Deliver secure, reliable, high-performance, and cost-effective compute infrastructure to meet demanding business needs.
- Access the on-demand infrastructure and capacity you need to run HPC applications faster and cost-effectively.
- Access environments in minutes, dynamically scale capacity as needed, and benefit from AWS’s pay-as-you-go pricing.
- Deliver the broadest choice of compute, networking (up to 400 Gbps), and storage services purpose-built to optimize price performance for ML projects.

---

## EC2 Instance Types

Recommended to follow [AWS EC2 Instance-Types (UserGuide)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-types.html) page for very detailed and updated information.


### General purpose

- General Purpose instances are designed to deliver a balance of compute, memory, and network resources. They are suitable for a wide range of applications, including web servers, small databases, development and test environments, and more.

### Compute optimized

- Compute Optimized instances provide a higher ratio of compute power to memory. They excel in workloads that require high-performance processing such as batch processing, scientific modeling, gaming servers, and high-performance web servers.

### Memory optimized

- Memory Optimized instances are designed to handle memory-intensive workloads. They are suitable for applications that require large amounts of memory, such as in-memory databases, real-time big data analytics, and high-performance computing.

### Storage optimized

- Storage Optimized instances are optimized for applications that require high, sequential read and write access to large datasets. They are ideal for tasks like data warehousing, log processing, and distributed file systems.

### Accelerated computing

- Accelerated Computing Instances typically come with one or more types of accelerators, such as Graphics Processing Units (GPUs), Field Programmable Gate Arrays (FPGAs), or custom Application Specific Integrated Circuits (ASICs). These accelerators offload computationally intensive tasks from the main CPU, enabling faster and more efficient processing for specific workloads.

--- 

![image](https://github.com/iam-veeramalla/aws-devops-zero-to-hero/assets/43399466/fc8e083c-dba5-41a6-94b9-14ebef0255c1)

---

### Instance families

```
    C – Compute

    D – Dense storage

    F – FPGA

    G – GPU

    Hpc – High performance computing

    I – I/O

    Inf – AWS Inferentia

    M – Most scenarios

    P – GPU

    R – Random access memory

    T – Turbo

    Trn – AWS Tranium

    U – Ultra-high memory

    VT – Video transcoding

    X – Extra-large memory
```

### Additional capabilities

```
    a – AMD processors

    g – AWS Graviton processors

    i – Intel processors

    d – Instance store volumes

    n – Network and EBS optimized

    e – Extra storage or memory

    z – High performance
```

---

## EC2 Instance Basics:

- Understanding the concept of virtual servers and instances.
Key components of an EC2 instance: AMI (Amazon Machine Image), instance types, and instance states.
- Differentiating between On-Demand, Reserved, and Spot instances.

## Launching an EC2 Instance:

- Step-by-step guide on launching an EC2 instance using the AWS Management Console.
- Configuring instance details, such as instance type, network settings, and storage options.
- Understanding security groups and key pairs for securing instances.

## Managing EC2 Instances:

- Starting, stopping, and terminating instances.
- Monitoring instance performance and utilization.
- Basic troubleshooting and accessing instances using SSH (Secure Shell).

---

# Jenkins Setup Guide on Ubuntu EC2 Instance

This guide walks you through the process of setting up **Jenkins** on an Ubuntu EC2 instance to facilitate continuous integration and continuous delivery (CI/CD) pipelines.

## Table of Contents
1. [Introduction to Jenkins]()
2. [Step 1: Checking Java Installation]()
3. [Step 2: Installing Jenkins]()
4. [Step 3: Configuring Security Group on AWS]()
5. [Step 4: Starting and Checking Jenkins Service]()
6. [Step 5: Unlocking Jenkins and Initial Setup]()
7. [Step 6: Setting Up Jenkins Administrator]()
8. [Conclusion]()

## Introduction to Jenkins
Jenkins is an open-source automation server that automates tasks such as building, testing, and deploying software. It is widely used for **CI/CD** pipelines and supports a variety of automation tasks.

## Step 1: Checking Java Installation
Jenkins requires Java to run. Ensure that Java is installed by running the following commands:

```bash
sudo apt update
sudo apt install fontconfig openjdk-17-jre
java -version
```

This will install Java and verify the installation.

## Step 2: Installing Jenkins
To install Jenkins, follow these steps:

```bash
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

This command sequence adds the Jenkins repository key, updates your package list, and installs Jenkins.

## Step 3: Configuring Security Group on AWS
Ensure that port **8080** is open in the security group of your EC2 instance to allow access to Jenkins via the web interface.

## Step 4: Starting and Checking Jenkins Service
Enable and start Jenkins using the following commands:

```bash
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
```

This ensures Jenkins starts automatically on boot.

## Step 5: Unlocking Jenkins and Initial Setup
Access Jenkins through your server's public IP and port **8080** (e.g., `http://your-ip-address:8080`). You'll be prompted to enter the initial administrator password.

Retrieve the password using the command:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Copy the password and paste it into the Jenkins web interface.

## Step 6: Setting Up Jenkins Administrator
Follow the on-screen instructions to create your Jenkins administrator account. You will need to provide a **username**, **password**, **full name**, and **email address**.

## Conclusion
🎉 **Congratulations!** You've successfully set up Jenkins on your Ubuntu EC2 instance. Jenkins is now ready to automate and streamline your CI/CD processes, enhancing your software development lifecycle.

---

![Jenkins Dashboard](https://github.com/user-attachments/assets/54fff5b4-db69-4f64-a17a-a77e3a9dc7ed)

---
