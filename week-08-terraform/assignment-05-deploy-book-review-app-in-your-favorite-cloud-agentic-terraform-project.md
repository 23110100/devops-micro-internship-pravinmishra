# Capstone Assignment — Deploy the Book Review App Using Terraform and Claude Code Agentic AI

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Student Details

**Full Name:** Add your full name here  
**Cloud Platform:** AWS or Azure  
**GitHub Repository URL:** Add your repository URL here  
**Public Application URL / Load-Balancer DNS:** Add the public URL or DNS here

---

## Purpose

Deploy the Book Review App using Terraform on AWS or Azure in a secure, highly available, production-style three-tier architecture. Use Claude Code, specialized subagents, Terraform MCP, and validation hooks to support the engineering workflow while keeping all infrastructure-changing operations under human control.

---

# Task 0 — Prepare the Project and Agentic AI Environment

## Goal

Prepare the Book Review App project and configure the provided Claude Code Agentic AI starter kit with project context, specialized subagents, Terraform MCP, validation hooks, and safety guardrails.

## Evidence

### Screenshot 1 — Project `CLAUDE.md`

Add a screenshot of the project `CLAUDE.md` showing the three-tier architecture, security boundaries, Terraform requirements, and human-approval rules.

![alt text](<Week 08 Assignment 5_Screenshort 1.png>)

---

### Screenshot 2 — Terraform Engineer Subagent

Add a screenshot showing the Terraform Engineer subagent configuration.

![alt text](<Week 08 Assignment 5_Screenshort 2.png>)

---

### Screenshot 3 — Architecture and Security Reviewer Subagent

Add a screenshot showing the Architecture and Security Reviewer subagent configuration.

![alt text](<Week 08 Assignment 5_Screenshort 3.png>)

---

### Screenshot 4 — Terraform MCP Connection

Add a screenshot showing Terraform MCP connected and available.

![alt text](<Week 08 Assignment 5_Screenshort 4.png>)

---

### Screenshot 5 — Validation Hooks

Add a screenshot showing the configured Claude Code validation hooks.

![alt text](<Week 08 Assignment 5_Screenshort 5.png>)

---

# Task 1 — Design the Three-Tier Architecture

## Goal

Design the required secure, highly available three-tier architecture and create an architecture diagram before building the infrastructure.

The diagram must show:

- VPC or VNet
- Availability Zones or equivalent availability locations
- Six subnets
- Internet connectivity
- NAT or outbound design
- Public load balancer
- Web Tier
- Internal load balancer
- Application Tier
- Managed MySQL
- Read replica
- Main traffic flow

## Architecture Diagram

[text](<../../Three-Tier Web Application Architecture.pdf>)

---

# Task 2 — Build the Terraform Networking and Security Layers

## Goal

Create the modular Terraform project and implement the network and security layers across the required public and private subnets.

## Evidence

### Screenshot 6 — Modular Terraform Project Structure

Add a screenshot showing the modular Terraform project structure.

![alt text](<Week 08 Assignment 5_Screenshort 6.png>)

---

### Screenshot 7 — Six-Subnet Architecture

Add a screenshot showing the six-subnet architecture across two availability locations.

![alt text](<Week 08 Assignment 5_Screenshort 7.png>)

---

### Screenshot 8 — Public and Private Tier Separation

Add a screenshot showing the public and private tier separation, including routing and security boundaries.

![alt text](<Week 08 Assignment 5_Screenshort 8.png>)

---

# Task 3 — Build the Load-Balancing and Compute Layers

## Goal

Deploy the public and internal load balancers and the Web and Application compute resources required by the Book Review App.

## Evidence

### Screenshot 9 — Web and Application Compute

Add a screenshot showing the Web and Application compute resources in their required subnets.

![alt text](<Week 08 Assignment 5_Screenshort 9.png>)

---

### Screenshot 10 — Public Load Balancer

Add a screenshot showing the internet-facing public load balancer.

![alt text](<Week 08 Assignment 5_Screenshort 10.png>)

---

### Screenshot 11 — Internal Load Balancer

Add a screenshot showing the private internal load balancer.

![alt text](<Week 08 Assignment 5_Screenshort 11.png>)

---

### Screenshot 12 — Healthy Targets

Add a screenshot showing healthy target groups or backend pools.

![alt text](<Week 08 Assignment 5_Screenshort 12.1png.png>)
![alt text](<Week 08 Assignment 5_Screenshort 12.png>)

---

# Task 4 — Build the Managed MySQL Database Layer

## Goal

Deploy a private, highly available managed MySQL database with a read replica and restrict database connectivity to the Application Tier.

## Evidence

### Screenshot 13 — Managed MySQL Database

Add a screenshot showing the managed MySQL database deployment.

![alt text](<Week 08 Assignment 5_Screenshort 13.png>)

---

### Screenshot 14 — High Availability

Add a screenshot showing the Multi-AZ or high-availability configuration.

![alt text](<Week 08 Assignment 5_Screenshort 14.png>)

---

### Screenshot 15 — Read Replica

Add a screenshot showing the read replica configuration.

![alt text](<Week 08 Assignment 5_Screenshort 15.png>)

---

### Screenshot 16 — Private Database Access

Add a screenshot showing that the database is private and accepts MySQL traffic only from the Application Tier.

![alt text](<Week 08 Assignment 5_Screenshort 16.png>)

---

# Task 5 — Validate, Review, and Apply the Terraform Configuration

## Goal

Validate the Terraform configuration, review the execution plan using both Agentic AI and human judgment, and apply the infrastructure changes only after all required checks pass.

## Evidence

### Screenshot 17 — Terraform Validation

Add a screenshot showing successful `terraform validate` output.

![alt text](<Week 08 Assignment 5_Screenshort 17.png>)

---

### Screenshot 18 — Terraform Plan

Add a screenshot showing the Terraform plan output.

![alt text](<Week 08 Assignment 5_Screenshort 18.png>)

---

### Screenshot 19 — Terraform Apply

Add a screenshot showing successful `terraform apply` completion.

![alt text](<Week 08 Assignment 5_Screenshort 19.png>)

---

# Task 6 — Deploy and Configure the Book Review Application

## Goal

Deploy and configure the Book Review App across the Web, Application, and Database tiers and verify the complete application functionality.

## Evidence

### Screenshot 20 — Homepage

Add a screenshot showing the Book Review App homepage through the public endpoint.

![alt text](<Week 08 Assignment 5_Screenshort 20.png>)

---

### Screenshot 21 — Login or Authentication

Add a screenshot showing successful login or authentication.

![alt text](<Week 08 Assignment 5_Screenshort 21.png>)

---

### Screenshot 22 — Book Data

Add a screenshot showing the book listing or book details.

![alt text](<Week 08 Assignment 5_Screenshort 22.png>)

---

### Screenshot 23 — Review Functionality

Add a screenshot showing the review functionality working successfully.

![alt text](<Week 08 Assignment 5_Screenshort 23.png>)

---

### Screenshot 24 — Backend or API Evidence

Add a screenshot showing that the backend or API is working successfully.

![alt text](<Week 08 Assignment 5_Screenshort 24.png>)

---

### Screenshot 25 — Database Reads and Writes

Add a screenshot showing successful database reads and writes.

![alt text](<Week 08 Assignment 5_Screenshort 25.png>)

## Public Application URL

**Public Application URL / DNS:** (http://bookreview-public-alb-79452117.us-east-2.elb.amazonaws.com/)

---

# Task 7 — Demonstrate the Agentic AI Workflow

## Goal

Demonstrate how Claude Code assisted with Terraform generation, architecture and security review, and evidence-based troubleshooting while infrastructure-changing decisions remained under human control.

You do not need to submit your complete Claude Code conversation history. Include only focused evidence.

## Evidence

### Screenshot 26 — AI-Assisted Terraform Generation

Add a screenshot showing one useful example of AI-assisted Terraform generation or improvement.

![alt text](<Week 08 Assignment 5_Screenshort 26.png>)

---

### Screenshot 27 — Architecture or Security Review

Add a screenshot showing one structured architecture or security review result.

![alt text](<Week 08 Assignment 5_Screenshort 27.png>)

---

### Screenshot 28 — AI-Assisted Troubleshooting

Add a screenshot showing one AI-assisted troubleshooting interaction based on collected evidence.

![alt text](<Week 08 Assignment 5_Screenshort 28.png>)

---

# Task 8 — Complete the Final Architecture Review

## Goal

Review the completed infrastructure against the original capstone requirements and resolve significant architecture, security, reliability, and cost issues.

Confirm that the final review covers:

- Tier separation
- Availability
- Public exposure
- Routing
- Security rules
- Load balancing
- Database privacy
- Secrets
- Terraform quality
- Module structure
- Reliability
- Obvious cost risks

Use Screenshot 27 as the focused evidence for the structured architecture or security review.

![alt text](<Week 08 Assignment 5_Screenshort 27-1.png>)

## Final Architecture Review Summary

The completed AWS Book Review infrastructure was reviewed against the capstone requirements for tier separation, availability, public exposure, routing, security rules, load balancing, database privacy, secrets management, Terraform quality, module structure, reliability, and cost. The core architecture is functioning end-to-end as **Internet → Public ALB → Web Tier → Internal ALB → Application Tier → private Amazon RDS MySQL**, with security-group-controlled traffic between tiers, multi-AZ deployment, Auto Scaling, private application/database resources, and SSM-based administration.

### Resolved Issues

The Web-tier launch-template/user-data failure was diagnosed and corrected, Terraform validation and planning were completed before deployment, and the subsequent Web Auto Scaling instance refresh reached 100%. Application-tier targets were verified healthy, book data and authentication/review APIs were successfully tested, and database read/write persistence was confirmed end-to-end. Core network segmentation, security-group chaining, private RDS placement, routing isolation, and SSM-based administration were also verified.

### Remaining Significant Issues

The final review identified several items requiring attention before treating the environment as production-hardened: possible database-password exposure in a local console artifact remains unconfirmed as remediated; Terraform state remains local, unencrypted, and unlocked; Web-tier instances retain public IP addresses despite being protected by security groups; the public application endpoint currently uses HTTP without TLS/HTTPS; and RDS lacks deletion protection and final-snapshot safeguards.

### Optional Hardening and Production Improvements

Additional improvements include tightening security-group egress rules, removing the unused duplicate module tree, enforcing LF line endings through `.gitattributes`, and reviewing the cost of continuously running two NAT gateways, two ALBs, Multi-AZ RDS, a read replica, and minimum-capacity EC2 instances when the capstone is not actively being demonstrated.
Overall, the capstone's core architecture is operational and has been validated through functional testing and evidence-based troubleshooting. The remaining findings primarily concern security hardening, state management, production safeguards, and cost optimization rather than basic application functionality.


---

# Task 9 — Answer the Reflection Questions

## Goal

Reflect on the architecture, Terraform implementation, and Agentic AI workflow. Answer each question briefly in your own words.

## Architecture

### 1. Why did you separate the Web, Application, and Database tiers?

I separated the tiers to give each part of the application a specific responsibility and security boundary. The Web tier handles user-facing traffic, the Application tier processes business logic and API requests, and the Database tier stores persistent data. This separation also makes the system easier to secure, scale, troubleshoot, and maintain.

### 2. Why is the Application Tier private?

The Application tier is private because users do not need direct access to the backend EC2 instances. Requests reach it through the Internal Application Load Balancer from the Web tier. This reduces the attack surface and allows the security group to accept application traffic only from the Internal ALB.

### 3. Why is MySQL private?

MySQL is private because the database should never be directly exposed to the internet. The RDS instance is deployed in private database subnets without an internet route, and its security group permits MySQL traffic on port 3306 only from the Application tier. This protects application data and database credentials.

### 4. Why are multiple Availability Zones used?

I used multiple Availability Zones to improve availability and remove dependence on a single AZ. The Web and Application Auto Scaling Groups span two AZs, the load balancers distribute traffic across them, and the RDS database uses Multi-AZ. If resources in one AZ become unavailable, resources in another AZ can continue serving the application.

### 5. What is the difference between Multi-AZ/high availability and a read replica?

Multi-AZ is mainly for high availability and failover. It maintains a standby database in another Availability Zone that can take over if the primary database fails. A read replica is mainly used to scale database reads by providing another database instance that applications can use for read operations. In my architecture, Multi-AZ improves resilience while the read replica provides additional read capacity.

## Terraform

### 6. How did you divide your Terraform into modules?

I divided the infrastructure by responsibility. My modules include network, security, public ALB, Web tier, internal ALB, Application tier, and RDS. This keeps the Terraform configuration organized and makes each infrastructure component easier to understand and maintain.

### 7. How do the modules communicate through variables and outputs?

Modules receive required values through input variables and expose useful resource information through outputs. The root module connects them together. For example, the network module outputs subnet and VPC IDs, which are passed into the Web, Application, security, load-balancer, and database modules. Load-balancer outputs are also passed to the tiers that need them.

### 8. What did you specifically check in `terraform plan`?

I checked which resources Terraform intended to add, change, or destroy and made sure the changes matched what I expected before applying them. During troubleshooting, I specifically verified a plan showing 0 to add, 1 to change, 0 to destroy, confirming that only the intended launch-template change would occur rather than unrelated infrastructure changes.

## Agentic AI

### 9. What was the purpose of `CLAUDE.md`?

CLAUDE.md provided Claude Code with project-specific instructions and context for the capstone. It helped define the intended architecture, Terraform approach, security expectations, and working rules so that AI assistance stayed aligned with the project instead of relying only on generic assumptions

### 10. What work did the Terraform Engineer subagent perform?

The Terraform Engineer subagent assisted with reviewing and developing the Terraform infrastructure, including the modular configuration for networking, security groups, load balancers, Auto Scaling tiers, and RDS. It helped analyze Terraform configuration while infrastructure-changing actions such as reviewing and approving terraform apply remained under my control.

### 11. What did the Architecture and Security Reviewer identify?

The reviewer confirmed strengths such as tier separation, multi-AZ availability, security-group chaining, private Application and RDS tiers, SSM-based administration, and restricted database access. It also identified remaining risks, including possible credential exposure in a console artifact, local Terraform state, public IPs on Web instances, lack of HTTPS, missing RDS deletion protection, permissive egress rules, and some unnecessary cost exposure.

### 12. Why did you use Terraform MCP instead of relying only on Claude's existing Terraform knowledge?

I used Terraform MCP to provide tool-based Terraform context rather than relying only on the model's existing knowledge. This helps ground AI assistance in Terraform-specific information and reduces the risk of using assumptions or outdated syntax when generating or reviewing infrastructure code.

### 13. What was the purpose of your validation hooks?

The validation hooks were used to catch problems before infrastructure changes were applied. They helped enforce checks such as Terraform formatting and validation so that syntax, formatting, and configuration problems could be detected early. I still reviewed the Terraform plan before approving infrastructure changes.

### 14. Describe one real issue Claude helped you troubleshoot.

Claude helped troubleshoot unhealthy Web-tier instances during an Auto Scaling instance refresh. We collected EC2 console output and decoded the launch-template user data. The evidence showed that cloud-init's scripts_user stage was failing before the bootstrap commands executed. Further investigation identified leading whitespace before the #!/bin/bash shebang caused by inconsistent Terraform heredoc indentation. I corrected the heredoc, validated and reviewed the Terraform plan, applied the change, decoded the new launch-template version to verify the fix, and the subsequent instance refresh reached 100%.

### 15. Describe one recommendation you reviewed, modified, or rejected instead of accepting blindly.

During troubleshooting, Claude initially suggested that CRLF line endings were the root cause of the Web-tier user-data failure. I did not accept that conclusion blindly. I provided stronger before-and-after evidence showing that the rendered script had leading whitespace before #!/bin/bash and that correcting the heredoc indentation fixed the problem. Claude then revised its analysis and downgraded CRLF to a secondary observation. This demonstrated that AI recommendations were treated as hypotheses to verify with evidence rather than automatically accepted.

---

# Task 10 — Publish the Mandatory LinkedIn Post

## Goal

Publish a LinkedIn post describing the capstone, the technical work completed, the Agentic AI workflow, and the lessons learned.

Write the post in your own words, include at least one project image or other proof, and ensure that it can be viewed by the submission reviewer.

## LinkedIn Post URL

**LinkedIn Post URL:** https://www.linkedin.com/feed/update/urn:li:share:7508366728402493441/

---

# Submission Instructions

- Complete Tasks 0–10 in sequence.
- Include all Screenshots 1–28 exactly as specified.
- Ensure that your full name is visible in the required screenshots.
- Include the selected cloud platform.
- Include the completed architecture diagram.
- Include the modular Terraform project structure.
- Include the working public application URL or public load-balancer DNS.
- Include all required Agentic AI workflow evidence.
- Answer all 15 reflection questions briefly in your own words.
- Include the published LinkedIn post URL.
- Do not expose cloud credentials, database passwords, SSH private keys, JWT secrets, access tokens, account IDs, Terraform state containing sensitive values, or other confidential information.
- Review all screenshots and project files carefully before submitting through GitHub.

---

# Completion Checklist

- [X] Selected AWS or Azure
- [X] Added and reviewed the Agentic AI starter files
- [x] Configured `CLAUDE.md`
- [x] Configured the Terraform Engineer subagent
- [x] Configured the Architecture and Security Reviewer subagent
- [x] Connected Terraform MCP
- [x] Configured validation hooks and safety guardrails
- [x] Created the architecture diagram
- [x] Created the six-subnet design
- [x] Configured public Web Tier routing
- [x] Kept the Application Tier private
- [x] Kept the Database Tier private
- [x] Configured tier-specific Security Groups or NSGs
- [x] Restricted backend port `3001`
- [x] Restricted MySQL port `3306` to the Application Tier
- [x] Created the public load balancer
- [x] Created the internal load balancer
- [x] Configured listeners and health checks
- [x] Deployed the Web Tier compute resources
- [x] Deployed the private Application Tier compute resources
- [x] Provisioned private managed MySQL
- [x] Configured Multi-AZ or high availability
- [x] Configured a read replica
- [x] Created the modular Terraform project
- [x] Used variables, outputs, and module dependencies
- [x] Used current Terraform documentation through MCP
- [x] Used hooks for deterministic validation
- [x] Completed `terraform fmt`
- [x] Completed `terraform validate`
- [x] Reviewed `terraform plan`
- [x] Completed the Terraform Engineer review
- [x] Completed the Architecture and Security review
- [x] Applied the infrastructure only after human approval
- [x] Deployed and configured the backend
- [x] Deployed and configured the frontend
- [x] Configured Nginx where required
- [x] Configured the internal backend endpoint
- [x] Configured the public frontend endpoint
- [x] Verified the homepage
- [x] Verified login or authentication
- [x] Verified book data
- [x] Verified review functionality
- [x] Verified the backend API
- [x] Verified database reads and writes
- [x] Verified healthy load-balancer targets
- [x] Included AI-assisted Terraform generation evidence
- [x] Included one architecture or security review
- [x] Included one AI-assisted troubleshooting example
- [x] Completed the final architecture review
- [x] Answered all 15 reflection questions
- [x] Published the mandatory LinkedIn post
- [x] Added the LinkedIn post URL
- [x] Captured all 28 required screenshots
- [x] Confirmed that my full name is visible in the required screenshots
- [x] Checked that no secrets or sensitive information are exposed

---

## About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory), focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations through hands-on experience.

---

## Resources

- Book Review App Repository: [https://github.com/pravinmishraaws/book-review-app](https://github.com/pravinmishraaws/book-review-app)
- DMI Official Website: [https://dmi.pravinmishra.com](https://dmi.pravinmishra.com)
- University: [https://university.pravinmishra.com](https://university.pravinmishra.com)
- Discord Community: [https://discord.pravinmishra.com](https://discord.pravinmishra.com)
- Blog: [https://dmi.pravinmishra.com/blog](https://dmi.pravinmishra.com/blog)
- YouTube Playlist: [https://www.youtube.com/playlist?list=PLFeSNDtI4Cho](https://www.youtube.com/playlist?list=PLFeSNDtI4Cho)
- Pravin Mishra on LinkedIn: [https://www.linkedin.com/in/pravin-mishra-aws-trainer/](https://www.linkedin.com/in/pravin-mishra-aws-trainer/)
- CloudAdvisory on LinkedIn: [https://www.linkedin.com/company/thecloudadvisory/](https://www.linkedin.com/company/thecloudadvisory/)

---

*This submission is part of the DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*
