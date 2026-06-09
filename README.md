# AWS Cloud Infrastructure Project: VPC EC2 and Security Controls

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

### Part 5: Creating a Security Group

The last step before launching an instance was to create a Security Group for the instance. A Security Group is essentially a virtual firewall that controls inbound and outbound traffic for a specific instance or any other resource. 
To create one, I went to the VPC dashboard and selected Security Groups, then "Create security group." I then configured the specifics for this security group. This included the name (Ubuntu-SSH-SG), the description (Allow SSH from my IP), and my VPC that I am associating it with. I then needed to create the inbound rule to specifiy with IP address would be able to use SSH to access the instance. The inbound rule is specified in the screenshot below (IM13). I then went back to the dashboard to confirm the Security Group was created (IM14). 

<img width="1305" height="800" alt="image" src="https://github.com/user-attachments/assets/7216dafe-35e2-4a38-8c90-8ed6dd2c2843" />

IM13: Creating a Security Group 

<img width="1705" height="160" alt="image" src="https://github.com/user-attachments/assets/9e5a9ec3-0fd7-4dfa-9a28-97201e52e661" />

IM14: Confirming that the Security Group was Created

### Part 6: Launching Ubuntu EC2 Instance

Now that my VPC was fully configured, I could launch my first instance in AWS. To launch one, I went to the search bar and typed "EC2" and opened the EC2 dashboard. I then selected launch instances to create one. 

1. **CONFIGURING INSTANCE.** The first step was configuring which OS and type to use. I chose Ubuntu and specifically the 26.04 LTS as it was the latest version and free tier eligible (IM15).
2. **CHOOSING INSTANCE TYPE.** For the type of Instance, I chose the t3.micro as it was free tier eligible (IM16).
3. **CREATING A KEY PAIR.** This needed to be created as it was needed for SSH authentication later on. I named it CyberLab-Key and securely stored it after downloading (IM17).
4. **CONFIGURING NETWORKING.** Now I needed to make sure this instance was in the correct network and subnet. I configured it to be under the VPC and subnet that was created earlier, as well as the security group (IM18).
5. **CONFIGURING STORAGE.** I left the storage as the default it came with, which was 8 GB (IM19).
6. **LAUNCHING INSTANCE.** Now that the instance was configured, I launched it and waited for the signal that it launched successfully (IM20). Once it did, I went to the EC2 dashboard to confirm it was created and running, as well as the public IP showing up (IM21).

<img width="972" height="760" alt="image" src="https://github.com/user-attachments/assets/a90ff2a2-dc68-4e41-941f-2ae91b5faebf" />

IM15: Configuring Instance OS 

<img width="966" height="177" alt="Screenshot 2026-06-07 110733" src="https://github.com/user-attachments/assets/372eb0c7-2901-4af9-91c9-085423b96b54" />

IM16: Configuring Instance Type

<img width="966" height="143" alt="Screenshot 2026-06-07 110739" src="https://github.com/user-attachments/assets/644a1118-e3f5-4ee9-9528-f796dfa745d7" />

IM17: Creating Key Pair for SSH Login

<img width="963" height="523" alt="image" src="https://github.com/user-attachments/assets/f24dd2de-551e-4057-a332-63a5616cebc4" />

IM18: Configuring Network Settings for Instance

<img width="967" height="401" alt="Screenshot 2026-06-07 110754" src="https://github.com/user-attachments/assets/3dab546d-adae-4124-ac7e-975587ef6dfc" />

IM19: Configuring Storage Settings (default) for Instance

<img width="1883" height="172" alt="Screenshot 2026-06-07 110916" src="https://github.com/user-attachments/assets/13c3f544-a12c-4f1c-8ed4-8a72978b61d2" />
<img width="1665" height="160" alt="Screenshot 2026-06-07 110959" src="https://github.com/user-attachments/assets/9924d31c-b3c8-4318-a302-84decf1c85b9" />

IM20&21: Instance Created and running with Public IP

### Part 7: Using SSH to Launch the Instance

Now that the Instance was created, I needed to get into it. I needed to use SSH using the key pair that was created earlier to reach it. First, I opened up PowerShell on my host and navigated to my documents directory, where the key was, then used the dir command to confirm the file exists, which it did. (IM22). I then ran the command "ssh -i .\CyberLab-Key.pem ubuntu@98.86.166.22" to log in. This command uses SSH and the key pair to log in to the Ubuntu instance with the public IP address associated with the instance. Once I ran the command, I was logged in to the Ubuntu instance with the username as the IP it gave me within the subnet (IM23). To confirm everything was set up correctly, I ran a few commands, including whoami, which returned ubuntu, hostname, which returned the same IP as my username, and ip a to confirm the network interfaces were correct (IM24).

<img width="534" height="222" alt="Screenshot 2026-06-07 111529" src="https://github.com/user-attachments/assets/93523ced-307d-4c6e-9ec0-dfa228a73442" />

IM22: confirming that the Key Pair file Exists

<img width="594" height="451" alt="Screenshot 2026-06-07 112456" src="https://github.com/user-attachments/assets/478d140a-47fe-49e0-859b-2537334fd194" />

IM23: Using SSH to Log in to Ubuntu Instance

<img width="754" height="319" alt="Screenshot 2026-06-07 113029" src="https://github.com/user-attachments/assets/51d0009a-dbe2-42d5-bb34-332a20434334" />

IM24: Running Basic Linux Commands to Confirm Correct Setup

### Part 8: Configuring Ubuntu Instance with Basic Linux Administration

Now that I was in the instance, I wanted to install some basic tools and run some other commands within the instance. The main things that I did were run the commands "sudo apt update" and "sudo apt upgrade -y" to install the latest package info and latest updates for the system (IM25). I also installed some network and web tools that are common commands within Linux as well. 

<img width="909" height="463" alt="Screenshot 2026-06-07 113807" src="https://github.com/user-attachments/assets/5cabe42f-b3a0-4bf2-93f9-b7de795f7385" />

IM25: Installing Latest Packages and Updates Within Ubuntu Instance

### Part 9: Testing Security Group

The last part of this Project was to see if the security Group I created was working correctly. To test this, I temporarily disabled the inbound rule that I created earlier and tried to connect via SSH. As expected, it failed to connect. I then re-enabled the rule and tried connecting again, and it worked, confirming that the rule was working as intended (IM26). 

<img width="595" height="447" alt="Screenshot 2026-06-07 114401" src="https://github.com/user-attachments/assets/f29590ba-7d23-4ef0-a5f9-1e2ee792ffbc" />

IM26: Testing Security Group Rule for SHH Log in

## Conclusion

This lab was successful in demonstrating the foundational concepts of AWS cloud networking, infrastructure deployment, and cloud security through the creation of a custom cloud environment. Throughout the project, key networking components were configured, including a Virtual Private Cloud (VPC), public subnet, Internet Gateway, and route table, providing a practical understanding of how network connectivity is established within AWS. These configurations helped reinforce how traditional networking concepts are applied in cloud environments.

In addition to building the network infrastructure, this project provided hands-on experience with deploying and managing an Ubuntu EC2 instance. Secure remote administration was implemented using SSH key-based authentication and Security Groups configured with least-privilege access controls. Testing Security Group rules further demonstrated how cloud firewalls regulate inbound traffic and protect cloud resources from unauthorized access.

The Linux administration portion of the lab introduced basic server management tasks, including system updates, package installation, and network connectivity verification. These activities helped bridge cloud infrastructure concepts with practical system administration skills commonly used in enterprise environments.

Overall, this project strengthened my understanding of AWS networking services, EC2 deployment, cloud security controls, and Linux administration. It provided valuable experience designing and securing a basic cloud infrastructure while establishing a foundation for future projects involving advanced AWS networking, cloud monitoring, server hardening, and security-focused cloud architectures.




















































