# aws-certification-journey

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


## Summary

By using a VPC, you can control the entire networking environment of your AWS resources, ensuring security, scalability, and reliability, much like managing an office building with various rooms and security measures. This setup helps you to create a robust and secure infrastructure for your applications.
