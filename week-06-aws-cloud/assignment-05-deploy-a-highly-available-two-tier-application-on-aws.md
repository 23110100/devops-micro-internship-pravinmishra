# Assignment 5 — Deploy a Highly Available Two-Tier Application on AWS (VPC + ALB + ASG + Multi-AZ RDS)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will design and deploy a highly available two-tier web application on AWS: highly available networking across two Availability Zones, an Application Load Balancer, an Auto Scaling Group for the web tier, and a private Multi-AZ RDS database. You must prove high availability with real failure tests.

---

# Task 1 — Create HA Networking (VPC + 4 Subnets + IGW + NAT + Route Tables)

## Goal

Build a VPC (10.0.0.0/16) with two public and two private subnets across two Availability Zones, an Internet Gateway, a NAT Gateway, and the matching public/private route tables.

### Evidence

#### Screenshot 1 — VPC details showing CIDR 10.0.0.0/16

![alt text](<Week 06 Assignment 05_Screenshot 1.png>)

---

#### Screenshot 2 — Subnets list showing four subnets and their Availability Zones

![alt text](<Week 06 Assignment 05_Screenshot 2.png>)
![alt text](<Week 06 Assignment 05_Screenshot 2.1png.png>)

---

#### Screenshot 3 — Public route table showing the Internet Gateway route and both public-subnet associations

![alt text](<Week 06 Assignment 05_Screenshot 3.png>)

---

#### Screenshot 4 — Private route table showing the NAT Gateway route and both private-subnet associations

![alt text](<Week 06 Assignment 05_Screenshot 4.png>)

---

#### Screenshot 5 — NAT Gateway status showing Available and the Elastic IP

![alt text](<Week 06 Assignment 05_Screenshot 5.png>)

---

# Task 2 — Create Security Groups (ALB, EC2, RDS) with Least Privilege

## Goal

Create `ha-alb-sg` (HTTP public), `ha-web-sg` (HTTP only from `ha-alb-sg`, SSH from your IP), and `ha-db-sg` (database port only from `ha-web-sg`).

### Evidence

#### Screenshot 6 — ALB Security Group inbound rules

![alt text](<Week 06 Assignment 05_Screenshot 6.png>)

---

#### Screenshot 7 — EC2 Security Group inbound rules showing the ALB Security Group reference and SSH from your IP

![alt text](<Week 06 Assignment 05_Screenshot 7.png>)

---

#### Screenshot 8 — RDS Security Group inbound rule showing the database port allowed only from the EC2 Security Group

![alt text](<Week 06 Assignment 05_Screenshot 8.png>)

---

# Task 3 — Deploy Database Tier (RDS Multi-AZ in Private Subnets)

## Goal

Launch a private, Multi-AZ RDS database (MySQL or PostgreSQL) using the private DB Subnet Group and `ha-db-sg`.

### Evidence

#### Screenshot 9 — RDS summary showing Multi-AZ = Yes and Publicly accessible = No

![alt text](<Week 06 Assignment 05_Screenshot 9.png>)
![alt text](<Week 06 Assignment 05_Screenshot 9.1png.png>)

---

#### Screenshot 10 — RDS connectivity section showing the DB Subnet Group and Security Group

![alt text](<Week 06 Assignment 05_Screenshot 10.png>)
![alt text](<Week 06 Assignment 05_Screenshot 10.1png.png>)

---

# Task 4 — Build a Launch Template (User Data Installs App + Connects to DB)

## Goal

Create a Launch Template whose user data installs the web-server runtime, deploys the application, configures the database connection, and starts the required services.

### Evidence

#### Screenshot 11 — Launch Template details showing that user data exists, including a visible snippet

![alt text](<Week 06 Assignment 05_Screenshot 11.png>)

---

#### Screenshot 12 — A running instance created from the template showing that the application responds on port 80 through a local test or browser using its public IP

![alt text](<Week 06 Assignment 05_Screenshot 12.png>)
![alt text](<Week 06 Assignment 05_Screenshot 12.1png.png>)

---

# Task 5 — Create an Application Load Balancer (ALB) Across 2 Public Subnets

## Goal

Create an internet-facing ALB across both public subnets with an HTTP listener and a healthy instance target group.

### Evidence

#### Screenshot 13 — ALB details showing two public subnets in two Availability Zones

![alt text](<Week 06 Assignment 05_Screenshot 13.png>)

---

#### Screenshot 14 — Target group showing at least one healthy target

![alt text](<Week 06 Assignment 05_Screenshot 14.png>)

---

# Task 6 — Create Auto Scaling Group (ASG) in 2 Public Subnets

## Goal

Create an Auto Scaling Group from the Launch Template across both public subnets, with desired capacity 2, minimum 2, and maximum 4, registered to the ALB target group.

### Evidence

#### Screenshot 15 — Auto Scaling Group showing desired, minimum, and maximum capacity and the selected subnet Availability Zones

![alt text](<Week 06 Assignment 05_Screenshot 15.1png.png>)
![alt text](<Week 06 Assignment 05_Screenshot 15.png>)

---

#### Screenshot 16 — EC2 instances list showing two running instances in different Availability Zones

![alt text](<Week 06 Assignment 05_Screenshot 16.png>)

---

# Task 7 — Configure App to Use RDS + Validate Read/Write

## Goal

Confirm the application communicates with the RDS database through the ALB DNS name with at least one read and one write operation.

### Evidence

#### Screenshot 17 — Browser showing the application loaded through the ALB DNS name with the URL visible

![alt text](<Week 06 Assignment 05_Screenshot 17.png>)

---

#### Screenshot 18 — Proof of a database write through a UI message or database query output

![alt text](<Week 06 Assignment 05_Screenshot 18.png>)

---

# Task 8 — High Availability Tests (Must Do Both)

## Goal

Test A: terminate one web instance and confirm the Auto Scaling Group replaces it automatically without interrupting the ALB.

Test B: simulate an Availability Zone impact (stop, detach, or reduce desired capacity in one AZ) and confirm the application stays available.

### Evidence

#### Screenshot 19 — EC2 showing the terminated instance and the newly launched instance; timestamps are helpful

![alt text](<Week 06 Assignment 05_Screenshot 19.png>)

---

#### Screenshot 20 — Target group showing healthy targets after replacement

![alt text](<Week 06 Assignment 05_Screenshot 20.png>)

---

#### Screenshot 21 — Evidence that an instance was removed, detached, placed in Standby, or stopped in one Availability Zone

![alt text](<Week 06 Assignment 05_Screenshot 21.png>)

---

#### Screenshot 22 — Browser showing that the ALB DNS endpoint still works during the change

![alt text](<Week 06 Assignment 05_Screenshot 22.png>)

---

# Task 9 — Architecture and Test-Results Summary

## Goal

Summarize the VPC/subnet layout, the ALB and Auto Scaling Group setup, the private Multi-AZ RDS setup, and the results of both high-availability tests.

### Evidence

#### Screenshot 23 — A simple architecture diagram, which may be hand-drawn, or an AWS console overview showing the components

![alt text](<Week 06 Assignment 05_Screenshot 23.png>)

---

### Notes

Summarize the VPC and subnets across the two Availability Zones.

The application is deployed inside a custom VPC, ha-vpc, using the CIDR block 10.0.0.0/16. The VPC spans two Availability Zones, us-east-2a and us-east-2b. Each Availability Zone contains a public subnet for the web tier and a private subnet for the database tier. The public subnets use a route table with an Internet Gateway, while the private subnets are designed to use NAT for outbound access. This layout provides network separation and supports high availability across two Availability Zones.

Summarize the ALB and Auto Scaling Group setup.

An internet-facing Application Load Balancer named ha-web-alb is deployed across both public subnets. It listens for HTTP traffic on port 80 and forwards requests to the ha-web-tg target group. The Auto Scaling Group, ha-web-asg, uses the HA-WEB-Launch-Template and distributes EC2 web servers across us-east-2a and us-east-2b. The ASG is configured with a desired capacity of 2, minimum capacity of 2, and maximum capacity of 4. The target group was verified with healthy instances, allowing the ALB to route traffic only to working web servers.

Summarize the private Multi-AZ RDS setup.

The MySQL RDS database ha-db is deployed using the ha-db-subnet-gp DB subnet group, which contains private subnets in both us-east-2a and us-east-2b. The database is not publicly accessible, and its security group permits MySQL traffic only from the web-tier security group. The EC2 application connects to RDS using its private RDS endpoint and the appdb database. In this deployment, Multi-AZ standby was not enabled because the account/plan did not make that option available, although the DB subnet group itself spans two Availability Zones and is ready for a Multi-AZ configuration if the account supports it.

Summarize the results of both high-availability tests.

For Test A, one running EC2 web instance was deliberately terminated. The Auto Scaling Group detected the loss and automatically launched replacement instance i-07d8cd4ddb491706b, restoring the configured desired capacity of two instances. This demonstrated automatic instance recovery by the ASG.

For Test B, the EC2 web instance in us-east-2a was stopped to simulate an Availability Zone/web-server failure. The second instance in us-east-2b remained available, and the WordPress application continued to load successfully through http://ha-web-alb-387970596.us-east-2.elb.amazonaws.com. This demonstrated that the ALB could continue serving the application from the healthy Availability Zone during the simulated failure.

---

# LinkedIn Post (Required)

## Goal

Publish a LinkedIn post about the high-availability build, including the ALB URL (or a redacted screenshot), three to five lines on what you built and how you tested high availability, and one proof screenshot.

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://www.linkedin.com/feed/update/urn:li:activity:7497562956096753664/`

---

#### Screenshot of LinkedIn post

![alt text](<Week 06 Assignment 05_Screenshot 24.png>)

---

# Submission Instructions

- Add all required screenshots in your submission
- Do not expose passwords, connection strings, private keys, or account IDs

---

# Completion Checklist

- [x] Task 1: VPC, four subnets, IGW, NAT Gateway, and route tables created (Screenshots 1–5)
- [x] Task 2: Least-privilege ALB, EC2, and RDS security groups created (Screenshots 6–8)
- [x] Task 3: Private Multi-AZ RDS created (Screenshots 9–10)
- [x] Task 4: Self-configuring Launch Template created and tested (Screenshots 11–12)
- [x] Task 5: ALB created across both public subnets (Screenshots 13–14)
- [x] Task 6: Auto Scaling Group running two instances across two AZs (Screenshots 15–16)
- [x] Task 7: Application verified through the ALB with a database read and write (Screenshots 17–18)
- [x] Task 8: Both high-availability tests completed (Screenshots 19–22)
- [x] Task 9: Architecture and test-results summary completed (Screenshot 23 & Notes)
- [x] LinkedIn post published and URL submitted
- [x] No sensitive data exposed

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme  
- 🎓 University: https://university.pravinmishra.com?utm_source=github&utm_medium=readme  
- 💬 Discord Community: https://discord.pravinmishra.com?utm_source=github&utm_medium=readme  
- 📝 Blog: https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*