# AWS Cloud Infrastructure Project VPC EC2 and Security Controls

## Objective

This lab/project aims to develop foundational skills in AWS cloud networking, infrastructure deployment, and security by building a basic cloud environment from the ground up. In this lab, I created a custom VPC, configured a public subnet, deployed an Internet Gateway, and set up route tables to establish internet connectivity within AWS. I then implemented security controls by creating a Security Group that restricted SSH access to my public IP address, following the principle of least privilege. After configuring the network infrastructure, I launched an Ubuntu EC2 instance, connected to it using SSH key-based authentication, and performed basic Linux administration tasks such as updating the system and installing networking utilities. I also tested Security Group functionality by removing and restoring SSH access to observe how cloud firewalls control inbound traffic. This project provided hands-on experience with VPCs, subnets, route tables, Internet Gateways, Security Groups, EC2 deployment, SSH authentication, and Linux administration, while demonstrating how traditional networking concepts are applied in cloud environments.

### Skills Learned

This project provided hands-on experience with the following cloud, networking, and security skills:

- Amazon VPC creation and cloud network architecture design
- Subnet creation and IP addressing within AWS environments
- Internet Gateway deployment and route table configuration
- Understanding how cloud networking maps to traditional networking concepts
- Understanding stateful cloud firewall behavior through Security Group testing
- Secure remote administration using SSH key-based authentication

### Tools Used

- Amazon Web Services (AWS Free Tier)
- AWS Management Console (Web-Based Interface)
- Amazon VPC
- Amazon EC2
- AWS Security Groups
- Ubuntu Server LTS
- SSH (Secure Shell)
- Windows PowerShell
- Web Browser (Chrome)

## Current Lab Environment Architecture/Setup

Below are screenshots of my lab Architecture before and after this week's lab/project (IM1&2). There were no new hardware or VM additions to the main lab environment, but I worked with AWS again this week, this time building a network within the cloud. The architecture for that environment can be seen below. 

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/3f5f5ca7-644a-4103-85c5-1798df835151" />
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/bd00c03e-ed4e-4e3b-a065-6b5a1683fc88" />

IM1&2: Current Lab Environment/Architecture Before and After this Week's Lab/Project

## Steps/Procedure

### Part 1: Creating a VPC

The first step in this project was to create a VPC (Virtual Private Cloud). This is the network with the cloud environment. To create one, I went to the VPC dashboard and clicked "Create VPC" in the top right corner. I was then met with the configuration options for the VPC. The main things I needed to configure were the name of the VPC (CyberLab-VPC) and the IPv4 CIDR block for the VPC (10.0.0.0/16). I left everything else as default (IM3). After creating it, I could now see my VPC listed in the dashboard.

<img width="1000" height="700" alt="image" src="https://github.com/user-attachments/assets/08bdab29-7bde-4e22-a1d2-db33404eeaf0" />

IM3: Creating a VPC for my Cloud Network

### Part 2: Creating Public Subnet 

Next was to create a public subnet for my VPC. This was needed because it serves as a segment within the network, thus holding different resources compared to other subnets. To create this, in the VPC dashboard, I selected the section "Subnets" and after that clicked "Create Subnet." To configure the subnet, I had to choose the VPC that I wanted to put it under, which is the one I just created. I then named it "Public-Subnet." I chose the availability zone closest to me (us-east-1a), and made a subnet block for the subnet (10.0.1.0/24). I then created the subnet (IM4) and confirmed that it was listed under my subnets (IM5). 

<img width="900" height="650" alt="image" src="https://github.com/user-attachments/assets/6dcb1184-444e-4e4a-b3bd-d8e79803b2a9" />

IM4: Creating a Subnet Under my VPC

<img width="1664" height="199" alt="Screenshot 2026-06-07 103756" src="https://github.com/user-attachments/assets/06b0c219-095d-4257-81c4-32c45da8a73a" />

IM5: Confirming Subnet was Created and is Under my VPC

### Part 3: Creating an Internet Gateway

Next up was to create an internet gateway for the VPC. An internet gateway is what allows resources (in this case, an EC2 instance) to communicate with the internet. Without it, even with a public IP address, the resources won't be able to communicate with the internet. To create it, I went to Internet Gateways under the VPC dashboard and selected "Create internet gateway." I named it "CyberLab-IGW" and created it (IM6). Now that it was created, I had to attach it to the VPC, because without doing that, it would be a good as useless because it wasn't connected to any specific network. To do this, I clicked on the IG and went to actions and selected "Attach to VPC (IM7)." I then attached it to the VPC I created, and it was configured (IM8). 

<img width="1424" height="463" alt="image" src="https://github.com/user-attachments/assets/8750b9ab-e436-45bf-b8f5-21d2d4f8a74b" />

IM6: Creating Internet Gateway

<img width="1648" height="185" alt="image" src="https://github.com/user-attachments/assets/7b2a6fb3-b632-4da1-9735-f452251e8c53" />

IM7: Attaching IG to VPC

<img width="1410" height="289" alt="Screenshot 2026-06-07 104009" src="https://github.com/user-attachments/assets/796019eb-bbdb-4681-95f0-364fb69abd65" />

IM8: IG attached to VPC

### Part 4: Creating and Configuring Route Table

The next step was to create a route table. A route table tells AWS where network traffic should go. Without it, traffic wouldn't know how to reach the internet. To create one, I went to "Route Tables" under the VPC dashboard and selected "Create route table." I then configured the route table by naming it "Public-RT" and attaching it to my VPC (IM9). I then needed to add the internet route so the route table knew specifically what to route to. I added the route "0.0.0.0/0" and the target to be my IG. This means that the route table should send all internet-bound traffic to the IG (IM10). Lastly, I needed to associate the route table with my subnet. This needed to be done because a route table doesn't automatically apply to every subnet in a VPC, meaning that if I don't associate it with my subnet, it won't know where to route traffic to. To do this, I went to my route table and clicked the "Subnet Associations" tab. I then clicked the checkbox for my subnet to associate it, and then confirmed that it was associated (IM11&12). 

<img width="1631" height="549" alt="Screenshot 2026-06-07 104225" src="https://github.com/user-attachments/assets/f3b405ff-288d-4ac4-b5f8-29c418ce21ee" />

IM9: Creating a Route Table Within my VPC

<img width="1638" height="346" alt="image" src="https://github.com/user-attachments/assets/f01ffeb4-5959-4e68-82d9-4db3942f759f" />

IM10: Adding Internet Route Within the Route Table

<img width="1642" height="411" alt="Screenshot 2026-06-07 104919" src="https://github.com/user-attachments/assets/572f1846-58b1-466f-bb7b-5e489aadd76e" />
<img width="1640" height="681" alt="Screenshot 2026-06-07 104957" src="https://github.com/user-attachments/assets/067eca1a-fa2c-4f5f-8aa8-dded6fb2f714" />

IM11&12: Configuring & Confirming that the Route Table is Associated with the Subnet

























































