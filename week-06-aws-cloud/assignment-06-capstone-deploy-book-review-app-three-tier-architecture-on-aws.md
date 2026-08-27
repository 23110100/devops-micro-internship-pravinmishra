# Assignment 6 — Capstone Assignment — Deploy Book Review App (Three-Tier Architecture) on AWS

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

This is the most important assignment of the course. You will deploy the Book Review App in a fully production-style three-tier architecture on AWS: a Next.js Web Tier behind Nginx and a public ALB, a private Node.js/Express App Tier behind an internal ALB, and a private Multi-AZ MySQL RDS database with a read replica. You are expected to design, deploy, isolate, debug, and document the result independently.

---

# Task 1 — Architecture Diagram

## Goal

Create an architecture diagram showing the custom VPC (10.0.0.0/16), the six subnets across two Availability Zones (two public Web Tier, two private App Tier, two private Database Tier), the public ALB, Web Tier EC2/Nginx, internal ALB, private App Tier EC2, private Multi-AZ RDS with its read replica, and the permitted traffic flow.

### Evidence

#### Diagram image or link

![alt text](<Book Review App - Three-Tier Architecture on AWS.png>)

---

# Task 2 — AWS Region & Services Used

## Goal

Record the AWS Region used and list every AWS service used across networking, compute, load balancing, security, and the database.

### Notes

**Region:**

US East (Ohio) — us-east-2

The architecture is distributed across:

Availability Zone A: us-east-2a
Availability Zone B: us-east-2b


---

**Services:**

Networking: Amazon VPC, six subnets across two Availability Zones, Internet Gateway, route tables, and Network ACLs.

Compute: Amazon EC2 for the Web Tier running Nginx + Next.js, and EC2 for the private App Tier running Node.js + Express.

Load Balancing: Elastic Load Balancing using an internet-facing Application Load Balancer (ALB) for the Web Tier and an internal ALB for communication between the Web and App tiers.

Security: Amazon EC2 Security Groups to restrict traffic between the public ALB, Web Tier, internal ALB, App Tier, and database tier.

Database: Amazon RDS for MySQL, configured for Multi-AZ high availability, plus an RDS Read Replica for read scaling.

High Availability / Scaling: EC2 Auto Scaling and Launch Templates can be used for the Web and App tiers if required by the deployment tasks.

---

# Task 3 — Public Entry Point

## Goal

Confirm the Book Review App loads through the public ALB DNS name.

### Evidence

#### Public ALB DNS

Paste your public ALB DNS name here:

c:\Users\Sammy\OneDrive\Desktop\Week 06\Week 06 Assignment 05_Screenshot 25.png

---

# Task 4 — Evidence Screenshots

## Goal

Capture visual proof of every tier and load balancer.

### Evidence

#### Web EC2

![alt text](<Week 06 Assignment 05_Screenshot 26.png>)

---

#### App EC2

![alt text](<Week 06 Assignment 05_Screenshot 26.1png.png>)

---

#### Public ALB

![alt text](<Week 06 Assignment 05_Screenshot 26.2png.png>)

---

#### Internal ALB

![alt text](<Week 06 Assignment 05_Screenshot 26.3png.png>)

---

#### RDS + Replica

![alt text](<Week 06 Assignment 05_Screenshot 26.4png.png>)

---

#### App UI proof

![alt text](<Week 06 Assignment 05_Screenshot 26.5.png>)
![alt text](<Week 06 Assignment 05_Screenshot 26.6.png>)
![alt text](<Week 06 Assignment 05_Screenshot 26.7png.png>)

---

# Task 5 — Summary

## Goal

Summarize what worked in the final deployment, the issues encountered and how each was fixed, and the tools or sources used to research and debug.

### Notes

**What worked:**

The final three-tier Book Review application was successfully deployed in AWS. The Web Tier ran the Next.js frontend on an EC2 instance behind a public Application Load Balancer. The App Tier ran the Node.js/Express backend on a private EC2 instance behind an internal Application Load Balancer. The backend successfully connected to a private Amazon RDS MySQL database and created the required schema and sample data. Both target groups eventually reported their EC2 targets as healthy. The final application was accessible through the public ALB DNS name and successfully displayed book data retrieved through the complete Web → App → Database architecture.

---

**Issues + fixes:**

Several issues occurred during deployment and testing. The App EC2 target initially showed as unhealthy because the backend was not running on port 3001 and the required App Tier security group was not correctly associated with the instance. The correct security group was attached and the backend application was deployed and configured.

The private App EC2 could not initially access GitHub because its private subnet had no internet route. A public NAT Gateway and Elastic IP were created, and the private subnet route table was updated so the instance could access the internet.

The backend was initially configured to use localhost for MySQL. An Amazon RDS MySQL database was created in the VPC, and the backend .env file was updated with the RDS endpoint, database name, credentials, and port 3306. After Node.js and npm were installed, the backend successfully connected to RDS using SSL and started on port 3001.

Another problem occurred because manually started frontend and backend processes stopped when SSH sessions ended. PM2 was installed on both EC2 instances so the Next.js frontend and Express backend could continue running independently of the SSH sessions.

The public application also returned 502 Bad Gateway because Nginx could not reach the stopped Next.js process. After the frontend was restarted with PM2, Nginx successfully returned HTTP 200.

Finally, the page loaded but displayed “No books available.” The frontend was attempting to use the wrong API routing configuration. Nginx was configured to proxy /api/ requests to the internal ALB, and the frontend's NEXT_PUBLIC_API_URL was changed to /api. The frontend was rebuilt and restarted. After this change, the application successfully displayed the sample books through the complete three-tier architecture.

---

**Tools/sources used:**

The main tools used for deployment and troubleshooting were the AWS Management Console, EC2 Instance Connect/SSH, Linux command-line utilities, curl, Nginx, Node.js/npm, and PM2. AWS EC2 target-group health checks were used to diagnose communication problems between the load balancers and instances. Commands such as curl, ss, ps, and nginx -t were used to test ports, HTTP responses, running processes, and Nginx configuration. GitHub was used to obtain the Book Review application source code. ChatGPT was also used as a troubleshooting and research aid to interpret AWS configuration issues, HTTP 502 errors, networking problems, RDS connectivity, process-management problems, and API routing issues.

---

# LinkedIn Post (Required)

## Goal

Publish a LinkedIn post sharing the capstone deployment, including the public ALB DNS (or a redacted screenshot), three to five lines on what you built and why it is production-style, and one proof screenshot.

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

[`Add your URL here`](https://www.linkedin.com/feed/update/urn:li:activity:7498072667107840001/)

---

#### Screenshot of LinkedIn post

![alt text](<Week 06 Assignment 05_Screenshot 26.8png_LinkedIn.png>)

---

# Submission Instructions

- Add all required screenshots and links in your submission
- Do not expose passwords, RDS credentials, connection strings, private keys, or account IDs

---

# Completion Checklist

- [x] Task 1: Architecture diagram completed
- [x] Task 2: AWS Region and services documented
- [x] Task 3: Public ALB DNS confirmed working
- [x] Task 4: All six evidence screenshots captured (Web Tier, App Tier, both ALBs, RDS + replica, app UI)
- [x] Task 5: Deployment summary completed (what worked, issues/fixes, tools/sources)
- [x] LinkedIn post published and URL submitted
- [x] App Tier and Database Tier confirmed not publicly accessible
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