# **Virtual Private Cloud**
- **VPC** = Virtual Private Cloud; **private network** to deploy your resources **(regional resource)**
- **Subnets** = allow you to partition your network inside your VPC (Availability Zone resource)
    - **A public subnet** is a subnet that is accessible from the internet
    - **A private subnet** is a subnet that is not accessible from the internet
- To define access to the internet and between subnets, we use **route tables**.
- **Internet Gateways =** enable internet connectivity for instances in a VPC.
- **Public Subnets =** are configured with a route to the Internet Gateway for external access.
- **NAT Gateways (managed by AWS) and NAT Instances (managed by the user)** provide internet access to instances in Private Subnets while keeping them private.
- **NACL (Network Access Control List):**
    - Acts as a firewall controlling traffic to and from a subnet.
    - Supports both ALLOW and DENY rules.
    - Attached at the subnet level.
    - Rules are based solely on IP addresses.
- **Security Groups:**
    - A firewall that controls traffic to and from an Elastic Network Interface (ENI) or an EC2 instance.
    - Only ALLOW rules are supported.
    - Rules can include IP addresses and other security groups.
# **VPC Connections**
- **VPC Peering allows two VPCs to connect privately using AWS' network**
    - Making them behave as if they were in the same network.
- It requires that the VPCs have non-overlapping CIDR blocks (IP address ranges).
- VPC Peering connections are not transitive; a direct connection must be established for each pair of VPCs that need to communicate.
    - If you have three VPCs (VPC A, VPC B, and VPC C), and you establish peering connections between A and B, and between B and C, VPC A and VPC C **cannot** communicate with each other through VPC B

- **VPC Endpoints enable private network connections to AWS services, bypassing the public internet for enhanced security and lower latency.**
- VPC Endpoint Gateway is used for S3 and DynamoDB
- VPC Endpoint Interface is used for other AWS service

- **AWS PrivateLink is the most secure and scalable way to expose a service to thousands of VPCs without requiring VPC peering, internet gateways, NAT devices, or route tables.**
- It requires a Network Load Balancer (NLB) in the service VPC and an Elastic Network Interface (ENI) in the customer VPC.

**Site-to-Site VPN:**
- Connects an **on-premises VPN to AWS.**
- The connection is **automatically encrypted.**
- Traffic goes over the **public internet.**
- On-premises: must use a **Customer Gateway (CGW)
- AWS: must use a **Virtual Private Gateway (VGW)**

**Direct Connect**
- Establishes a **physical connection between on-premises and AWS.**
    - The connection is private, secure, and fast.
    - Traffic goes over a private network.
    - Takes at least a month to establish

**AWS Client VPN (using OpenVPN):**
- Allows you to connect from your computer to your private network in AWS and on-premises.
- Enables you to connect to your EC2 instances over a private IP, as if you were in the private VPC network.
- The connection goes over the public internet.

 **Transit Gateway**
- Provides **transitive peering between thousands of VPCs and on-premises networks using a hub-and-spoke (star) connection.**
- Utilizes a single gateway to offer this functionality.
- Compatible with Direct Connect Gateway and VPN connections.

## How do you monitor the network?
- **VPC Flow Logs, Subnet Flow Logs, and Elastic Network Interface Flow Logs capture information about IP traffic going into your interfaces, aiding in monitoring and troubleshooting connectivity issues, such as:**
    - Subnets to the internet.
    - Subnets to subnets.
    - Internet to subnets.
- They also capture network information from AWS managed interfaces like Elastic Load Balancers, ElastiCache, RDS, and Aurora.
- VPC Flow Logs data can be sent to S3, CloudWatch Logs, and Kinesis Data Firehose for analysis and storage.