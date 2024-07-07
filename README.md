## Design and set up a Virtual Private Cloud (VPC) with public and private subnets. Implement routing, security groups, and network access control lists (NACLs) to ensure proper communication and security within the VPC. Work in the AWS EU-West-1 (Ireland) region.

## Link to Architectural Diagram
https://lucid.app/lucidchart/d1b12868-4942-44dd-a6eb-a2639c3e9bc7/edit?invitationId=inv_c8a5d7bb-0a33-48e1-aa84-17e6eb487a99&page=0_0#  

![ArchDiagram](https://github.com/Benn1440/DevOpsTask5/assets/67696393/b6a3cbbe-41ae-4a58-b167-f5009fecc96a)

# Insights into the Individual components
VPC (Virtual Private Cloud): A logically isolated section of the AWS cloud where you can launch AWS resources in a virtual network.
 # Subnets:
  * Public Subnet: A subnet with a route to an internet gateway, making its resources accessible from the internet.
  * Private Subnet: A subnet that does not have a direct route to the Internet.
  * Internet Gateway (IGW): A horizontally scaled, redundant, and highly available VPC component that allows communication between instances in KCVPC and the internet.
  *  NAT Gateway: Allows instances in a private subnet to connect to the internet or other AWS services, but prevents the internet from initiating a connection with those instances.
# Route Tables:
  * Public Route Table: Directs internet traffic to the IGW.
  *  Private Route Table: Routes internet traffic to the NAT Gateway.
# Security Groups:
Are Virtual firewalls that control inbound and outbound traffic for your instances.
   * Network ACLs (NACLs): Provide an additional layer of security for your VPC by controlling traffic to and from one or more subnets.
   * A network access control list (NACL) is an optional layer of security for your VPC that acts as a firewall for controlling traffic in and out of one or more subnets. You might set up network ACLs with rules similar to your security groups in 
    order to add a layer of security to your VPC.

# Create VPC:
  * Login to the AWS Console, search for VPC
  * Name: KCVPC
  * IPv4 CIDR block: 10.0.0.0/16
   <img width="721" alt="VPC" src="https://github.com/Benn1440/DevOpsTask5/assets/67696393/6464103a-7f57-4d39-ad1c-777afa158c4a">

# Create Subnets with the following Information:
 * Public Subnet:
   * Name: PublicSubnet
   * IPv4 CIDR block: 10.0.1.0/24
   * Availability Zone: Select anyone from your region
* Private Subnet:
   * Name: PrivateSubnet
   * IPv4 CIDR block: 10.0.2.0/24
   * Availability Zone: Select anyone from your region
  <img width="719" alt="Subnets" src="https://github.com/Benn1440/DevOpsTask5/assets/67696393/402755a1-434a-4b60-a0cd-96af2003e657">

# Configure an Internet Gateway (IGW):
   * Create and attach an IGW to KCVPC.
  <img width="1426" alt="IGW2VPC" src="https://github.com/Benn1440/DevOpsTask5/assets/67696393/40b1a2d6-8cde-46c5-a737-8529c0cc763b">

# Configure Route Tables:
 * Public Route Table:
 * Name: PublicRouteTable
 <img width="1422" alt="RT" src="https://github.com/Benn1440/DevOpsTask5/assets/67696393/ba807b39-7545-435d-abfe-ee3b65bc49d0">

*  Associate PublicSubnet with this route table.
*  Add a route to the IGW (0.0.0.0/0 -> IGW).
 <img width="1432" alt="PublicSNRT" src="https://github.com/Benn1440/DevOpsTask5/assets/67696393/67c7cd4e-6f46-4ef2-a5d2-2b9c739e7838">

 * Private Route Table:
* Name: PrivateRouteTable
* Associate PrivateSubnet with this route table.
* There should not be any direct route to the internet.

 <img width="720" alt="PrivateSNRT" src="https://github.com/Benn1440/DevOpsTask5/assets/67696393/54502399-c5bc-48ef-ba13-4c3b1544d0d4">

# Set Up Security Groups:
* Create a Security Group for public instances (e.g., web servers):
  * Allow inbound HTTP (port 80) and HTTPS (port 443) traffic from anywhere (0.0.0.0/0).
  * Allow inbound SSH (port 22) traffic from a specific IP (e.g., your local IP). (https://www.whatismyip.com/)
  * Allow all outbound traffic.
* Create a Security Group for private instances (e.g., database servers):
  * Allow inbound traffic from the PublicSubnet on required ports (e.g., MySQL port 3306).
  * Allow all outbound traffic.
   <img width="1422" alt="SG" src="https://github.com/Benn1440/DevOpsTask5/assets/67696393/c713f071-17b9-4792-8ce3-f0c68774bd17">
   
# Network ACLs:
  * Configure NACLs for additional security on both subnets.
  * Public Subnet NACL: Allow inbound HTTP, HTTPS, and SSH traffic. Allow outbound traffic.
  * Private Subnet NACL: Allow inbound traffic from the public subnet. Allow outbound traffic to the public subnet and internet.

<img width="1430" alt="NACL" src="https://github.com/Benn1440/DevOpsTask5/assets/67696393/8bc247e1-ee5c-471f-970b-ec31c955acf0">

# Deploy Instances:
 * Launch an EC2 instance in the public subnet:
 * Use the public security group.
 * Verify that the instance can be accessed via the internet.
 * Launch an EC2 instance in the private subnet:
 * Use the private security group.
 * Verify that the instance can access the internet through the NAT Gateway and can communicate with the public instance.

<img width="671" alt="Instances" src="https://github.com/Benn1440/DevOpsTask5/assets/67696393/b867ffb8-7b5f-46c9-becb-3705bf21dcbe">



