<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# VPC Peering

**Project Link:** [View Project](http://learn.nextwork.org/projects/aws-networks-peering)

**Author:** R  

---

## VPC Peering

![Image](http://learn.nextwork.org/serene_teal_majestic_duck/uploads/aws-networks-peering_88727bef)

---

## Introducing Today's Project!

### What is Amazon VPC?

Amazon VPC is a virtual network you can define in AWS, giving you full control over your networking setup like IP ranges, subnets, route tables, and gateways.

### How I used Amazon VPC in this project

I used Amazon VPC to create two separate networks and connected them using VPC peering to enable private communication between their EC2 instances.

### One thing I didn't expect in this project was...

I didn’t expect to troubleshoot ICMP traffic and update security group rules just to get a ping working. That was great hands-on learning!

### This project took me...

This project took me around 1 hour to complete, including setup, troubleshooting, and testing the VPC peering connection.

---

## In the first part of my project...

### Step 1 - Set up my VPC

In this step, we will create two VPCs using the AWS VPC wizard. This involves defining unique IPv4 CIDR blocks for each VPC to ensure non-overlapping IP ranges, which is essential for seamless connectivity and to avoid routing conflicts.

### Step 2 - Create a Peering Connection

​In this step, we will establish a VPC peering connection between our two VPCs to enable communication between them. This involves creating the peering connection, accepting the request, and updating route tables to allow traffic flow.

### Step 3 - Update Route Tables

In this step, we are updating the route tables of both VPCs to enable traffic routing between them through the established peering connection. This configuration allows resources in each VPC to communicate using their private IP addresses.

### Step 4 - Launch EC2 Instances

​In this step, we're launching an EC2 instance in each VPC to test the peering connection. By deploying instances in both VPCs, we can verify that they can communicate over their private IPs, confirming the peering setup is functioning correctly.

---

## Multi-VPC Architecture

I started my project by launching two VPCs, each with a single public subnet.

The CIDR blocks for VPCs 1 and 2 are 10.1.0.0/16 and 10.2.0.0/16, respectively. They have to be unique because VPC peering requires non-overlapping CIDR blocks to avoid routing conflicts and ensure successful peering connections.

### I also launched 2 EC2 instances

I didn't set up key pairs for these EC2 instances as AWS EC2 Instance Connect allows secure SSH access without the need for managing key pairs manually. This simplifies the connection process and eliminates the need to handle private keys.

![Image](http://learn.nextwork.org/serene_teal_majestic_duck/uploads/aws-networks-peering_11111111)

---

## VPC Peering

A VPC peering connection is a networking link between two VPCs that enables routing traffic between them using private IP addresses.

VPCs use peering connections to enable direct, private communication between resources in different VPCs without traversing the public internet.

The difference between a Requester and an Accepter in a peering connection is that the Requester initiates the peering request, while the Accepter receives and must approve the request for the connection to be established.

![Image](http://learn.nextwork.org/serene_teal_majestic_duck/uploads/aws-networks-peering_1cbb1b88)

---

## Updating route tables

After accepting a peering connection, my VPCs' route tables need to be updated because, by default, they don't know how to route traffic to the newly peered VPC. Adding specific routes ensures intended traffic goes through the peering connection.

My VPCs' new routes have a destination of the peer VPC's CIDR block. The routes' target was the VPC peering connection ID.

![Image](http://learn.nextwork.org/serene_teal_majestic_duck/uploads/aws-networks-peering_4a9e8014)

---

## In the second part of my project...

### Step 5 - Use EC2 Instance Connect

In this step, I'm connecting to my first EC2 instance using EC2 Instance Connect. This lets me securely access the instance's terminal from my browser, so I can run commands and later test communication with the second instance.

### Step 6 - Connect to EC2 Instance 1

In this step, I will attempt to reconnect to my EC2 instance using EC2 Instance Connect, now that the IP address issue has been resolved. If I encounter any further errors during this process, I will identify and address them accordingly.

### Step 7 - Test VPC Peering

I will test the VPC peering connection by sending test messages from Instance 1 in VPC 1 to Instance 2 in VPC 2. If any connection errors occur, I will troubleshoot and resolve them to ensure successful bidirectional communication between instances.

---

## Troubleshooting Instance Connect

Next, I used EC2 Instance Connect to access my instance directly through the AWS Management Console, simplifying the connection process without needing an SSH client.

I was stopped from using EC2 Instance Connect as my instance lacked a public IPv4 address, which is required for establishing the connection.

![Image](http://learn.nextwork.org/serene_teal_majestic_duck/uploads/aws-networks-peering_7685490c)

---

## Elastic IP addresses

To resolve this error, I set up Elastic IP addresses. Elastic IP addresses are static, public IPv4 addresses designed for dynamic cloud computing, masking instance or software failures by rapidly remapping the address to another instance.

Associating an Elastic IP address resolved the error because it provided my instance with a consistent public IPv4 address, enabling EC2 Instance Connect to establish a successful connection.

![Image](http://learn.nextwork.org/serene_teal_majestic_duck/uploads/aws-networks-peering_45663498)

---

## Troubleshooting ping issues

To test VPC peering, I ran the command ping [Instance 2's Private IPv4 address] from Instance 1's terminal.

A successful ping test would validate my VPC peering connection because it confirms that network traffic can flow between the two instances over their private IP addresses, indicating proper routing and security group configurations.

I had to update my second EC2 instance's security group because it was not allowing inbound ICMP traffic. I added a new rule that permits all ICMP - IPv4 traffic from the CIDR block of VPC 1 (10.1.0.0/16), enabling instances to communicate via ping.

![Image](http://learn.nextwork.org/serene_teal_majestic_duck/uploads/aws-networks-peering_7a29d352)

---

---
