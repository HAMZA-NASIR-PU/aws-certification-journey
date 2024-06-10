# My AWS Certification Journey

This README.md and the repository contain all the information addressing the doubts I am facing in my AWS Certification journey.

## <img src="https://user-images.githubusercontent.com/74038190/212257467-871d32b7-e401-42e8-a166-fcfd7baa4c6b.gif" width ="25" style="margin-bottom: -5px;"> What is a VPC?

A Virtual Private Cloud (VPC) is a virtual network dedicated to your AWS account. It is logically isolated from other virtual networks in the AWS cloud, allowing you to launch AWS resources, such as EC2 instances, in a defined virtual network.


### Real-Life Analogy

Think of a VPC as a private office building:

- **Office Building (VPC):** This is your own private space where you can control who enters, what rooms they can access, and how the internal infrastructure is organized.
- **Rooms (Subnets):** Within your office building, you have different rooms for different purposes. Some rooms might be for public access (public subnets), like a reception area, while others are for private use (private subnets), like an employee lounge or server room.
- **Doors and Security Guards (Security Groups and Network ACLs):** These control who can enter and exit the rooms. Security groups are like security guards for individual rooms, while Network ACLs (Access Control Lists) are like security at the building's perimeter.
- **Hallways (Route Tables):** These define how you move between rooms. Route tables determine how traffic is directed within the VPC.


### Setting Up a VPC: Real-Life Example

Imagine you are setting up a web application for a small business:

#### 1. Creating a VPC

You create a VPC with a specific IP address range, say `10.0.0.0/16`. This is like acquiring an office building with a certain number of rooms.

#### 2. Dividing the VPC into Subnets

You divide your VPC into subnets. For instance:

- **Public Subnet:** `10.0.1.0/24`
- **Private Subnet:** `10.0.2.0/24`

The public subnet is where your web servers will be accessible to the internet, like the reception area of your office. The private subnet is where your database servers will be, away from public access, like the employee-only areas.

#### 3. Setting Up Security

- **Security Groups:**
  - You create security groups to control traffic to your web servers and databases. For example, you might allow HTTP and HTTPS traffic to your web servers but restrict the database server to only accept traffic from the web servers.

- **Network ACLs:**
  - You set up Network ACLs to add an extra layer of security at the subnet level. For example, you might deny all inbound traffic by default and only allow specific IP addresses or ranges.

For more detailed information, visit the [AWS VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html).

#### 4. Route Tables

You create route tables to manage traffic routing within your VPC. For instance, you might have:

- A route table for the public subnet that routes internet traffic through an internet gateway.
- A route table for the private subnet that does not allow direct internet access, but allows internal traffic.

#### 5. Internet Gateway

To allow your public subnet to communicate with the internet, you attach an internet gateway to your VPC. This is like having a main entrance/exit door in your office building.


### Example Scenario

Your small business runs an online store:

- **Web Servers (Public Subnet):** These servers host your website and are accessible by customers from the internet.
  - IP Range: `10.0.1.0/24`
  - Security Group: Allows inbound traffic on port 80 (HTTP) and port 443 (HTTPS).

- **Database Servers (Private Subnet):** These servers store customer data and order information. They should not be directly accessible from the internet.
  - IP Range: `10.0.2.0/24`
  - Security Group: Only allows inbound traffic from the web servers on the database port (e.g., port 3306 for MySQL).

- **Networking:**
  - **Internet Gateway:** Provides internet access to your web servers.
  - **NAT Gateway (optional):** If your database servers need to access the internet for updates, you might use a NAT gateway.


### Summary

By using a VPC, you can control the entire networking environment of your AWS resources, ensuring security, scalability, and reliability, much like managing an office building with various rooms and security measures. This setup helps you to create a robust and secure infrastructure for your applications.


## <img src="https://user-images.githubusercontent.com/74038190/212257467-871d32b7-e401-42e8-a166-fcfd7baa4c6b.gif" width ="25" style="margin-bottom: -5px;"> Is Internet Gateway associated with Subnet or VPC in AWS ?

### Overview

In Amazon Web Services (AWS), an Internet Gateway is a horizontally scaled, redundant, and highly available VPC component that allows communication between instances in your VPC and the internet. This README provides an overview of how an Internet Gateway is associated within AWS infrastructure.

### Association of Internet Gateway

An Internet Gateway is **associated with a VPC (Virtual Private Cloud)**, not with a specific subnet. This association allows instances within the VPC to communicate with the internet.

#### Key Points:

- **VPC Association**: The Internet Gateway is attached to a VPC, providing the VPC with a route to the internet.
- **Subnet Configuration**: To enable instances within a subnet to access the internet, additional configurations are required at the subnet level.

### Configuring Internet Access for Subnets

While the Internet Gateway is associated with the VPC, subnets within the VPC require specific configurations to enable internet access:

1. **Route Table Configuration**:
   - The subnet's route table must have a route that directs internet-bound traffic (0.0.0.0/0) to the Internet Gateway.

2. **Public IP Addresses**:
   - Instances must have public IP addresses or Elastic IP addresses to be reachable from the internet.

3. **Network ACLs and Security Groups**:
   - Appropriate Network ACLs and Security Group rules must be configured to allow the necessary inbound and outbound traffic.

### Example Configuration

#### Step-by-Step:

1. **Attach Internet Gateway to VPC**:
   - In the AWS Management Console, navigate to the VPC dashboard.
   - Select "Internet Gateways" and create a new Internet Gateway.
   - Attach the Internet Gateway to your VPC.

2. **Configure Route Table**:
   - Navigate to the "Route Tables" section.
   - Select the route table associated with the subnet.
   - Edit the route table and add a new route:
     - **Destination**: 0.0.0.0/0
     - **Target**: [Internet Gateway ID]

3. **Assign Public IP to Instances**:
   - Ensure that instances in the subnet have public IP addresses assigned. This can be done at launch time or by associating an Elastic IP.

4. **Update Security Groups**:
   - Modify the security group associated with your instances to allow inbound traffic (e.g., HTTP, HTTPS, SSH).

### Important Considerations

- **Private Subnets**:
  - Instances in private subnets do not have direct internet access, even if the route table points to the Internet Gateway. To provide internet access, use a NAT Gateway or NAT instance in a public subnet.

- **Cost Management**:
  - Be mindful of Elastic IP address costs. AWS charges for unused Elastic IP addresses and for any Elastic IP addresses associated with stopped instances.

By following these guidelines, you can effectively manage internet access for your instances within a VPC using an Internet Gateway.

### Conclusion

An Internet Gateway is associated with a VPC, enabling internet communication for instances. Subnets within the VPC require specific configurations in their route tables and instances must have public IP addresses for internet access.

For more information, refer to the [AWS Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Internet_Gateway.html).

## <img src="https://user-images.githubusercontent.com/74038190/212257467-871d32b7-e401-42e8-a166-fcfd7baa4c6b.gif" width ="25" style="margin-bottom: -5px;"> Can you explain the use of Route Table ?

### Answer

A route table contains a set of rules, called routes, that are used to determine where network traffic from your subnet or gateway is directed. When a network packet arrives at a Router, it determines the destination IP address of a received packet and makes the routing decisions to send packet to their destination.


## <img src="https://user-images.githubusercontent.com/74038190/212257467-871d32b7-e401-42e8-a166-fcfd7baa4c6b.gif" width ="25" style="margin-bottom: -5px;"> What do you mean by redundancy in AWS ?

### Answer

In Amazon Web Services (AWS), redundancy is a strategy that involves duplicating hardware or software components to prevent single points of failure and increase a system's reliability and availability. This can help prevent data loss and downtime in the event of a failure. 
