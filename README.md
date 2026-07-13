# AWS VPC Peering Connectivity 
# 📌 Project Overview

This project demonstrates how to establish __secure private communication between two AWS Virtual Private Clouds (VPCs) using VPC Peering__.Two Ubuntu EC2 instances were launched in separate VPCs, Nginx was installed on both instances, and connectivity was verified using Ping and cURL over private IP addresses.

The project shows how VPC Peering enables communication through the AWS private network without routing traffic over the public internet.

# 🏗️ Architecture
                           AWS Cloud

               +-----------------------------+
               |          VPC - 1            |
               |      CIDR: 10.0.0.0/16      |
               |                             |
               |  Public Subnet              |
               |  EC2 Ubuntu                 |
               |  Private IP: 10.0.0.102     |
               +-------------+---------------+
                             |
                    VPC Peering Connection
                             |
               +-------------+---------------+
               |          VPC - 2            |
               |      CIDR: 10.1.0.0/16      |
               |                             |
               |  Public Subnet              |
               |  EC2 Ubuntu                 |
               |  Private IP: 10.1.0.200     |
               +-----------------------------+
# 🎯 Objective
Create two separate VPCs.
Connect both VPCs using VPC Peering.
Enable private communication between EC2 instances.
Verify network connectivity using Ping.
Verify application connectivity using cURL and Nginx.
# 🛠️ AWS Services Used
Amazon VPC

VPC Peering

Amazon EC2

Internet Gateway

Route Tables

Security Groups

Ubuntu Server

Nginx

# Implementation Steps
 # Step 1: Create VPC-A
 
Create a VPC
CIDR Block: 10.0.0.0/16

 # Step 2: Create VPC-B__
Create another VPC
CIDR Block: 10.1.0.0/16

# Step 3: Create Public Subnets

Create one subnet in each VPC.

Example:

VPC-1

10.0.0.0/24

 VPC-2

10.1.0.0/24

# Step 4: Create and Attach Internet Gateway

Create an Internet Gateway for each VPC.

Attach the IGW to the corresponding VPC.

_Note: Internet Gateways were required to provide internet access for package installation and SSH access. They are not required for VPC Peering traffic_.

# Step 5: Configure Route Tables

For both VPCs:

Add

0.0.0.0/0

Target

Internet Gateway

Associate the route table with the public subnet.

# Step 6: Launch EC2 Instances

Launch one Ubuntu EC2 instance in each VPC.

Example:

EC2-1
Ubuntu
Private IP: 10.0.0.102
EC2-2
Ubuntu
Private IP: 10.1.0.200

Enable Auto Assign Public IP.

# Step 7: Configure Security Groups

Allow the following inbound rules:

Type	Port	Source
SSH	22	My IP
HTTP	80	Peer VPC CIDR
ICMP	All	Peer VPC CIDR

# Step 8: Create VPC Peering Connection
Open VPC Console
Select Peering Connections
Create a new Peering Connection
Select Requester VPC
Select Accepter VPC
Accept the request

Status should become:

Active

# Step 9: Update Route Tables for Peering
VPC-1 Route Table

Destination

10.1.0.0/16

Target

VPC Peering Connection
VPC-2 Route Table

Destination

10.0.0.0/16

Target

VPC Peering Connection

# 💻 Configure Nginx

SSH into both EC2 instances.

Update packages

sudo apt update

Install Nginx

sudo apt install nginx -y

Start Nginx

sudo systemctl start nginx

Enable Nginx

sudo systemctl enable nginx

Verify

sudo systemctl status nginx

# 🔍 Testing Connectivity
*Test Network Connectivity*

From EC2-1

ping 10.1.0.200

Successful replies confirm private network connectivity.

Test HTTP Connectivity

From EC2-1

curl http://10.1.0.200

Expected Output

Welcome to nginx!

Similarly, test from EC2-2

curl http://10.0.0.102

This verifies two-way communication over the VPC Peering connection.

# 📚 Key Learnings
Created multiple VPCs with different CIDR blocks.

Configured Internet Gateways for internet access.

Established a VPC Peering connection.

Updated route tables for private routing.

Configured Security Groups for secure communication.

Installed and managed Nginx on Ubuntu.

Verified connectivity using Ping.

Verified application-level communication using cURL.

Learned the difference between internet routing and private AWS networking.

# 👩‍💻 Author

__Shraddha Masalge__

Aspiring Cloud & DevOps Engineer

Skills: AWS | Linux | Networking | Git | GitHub | Shell Scripting | Nginx
