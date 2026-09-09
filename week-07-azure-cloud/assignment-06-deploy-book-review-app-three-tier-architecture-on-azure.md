# Assignment 6 — Capstone: Deploy Book Review App (Three-Tier Architecture) on Azure

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

This is the most important assignment of the course. You will deploy the Book Review App in a production-ready, best-practice-compliant three-tier architecture on Azure: separated presentation, application, and database tiers, least-privilege network access, a controlled public entry point, protected secrets, and availability/monitoring evidence.

---

# Task 1 — Design the Azure Three-Tier Architecture

## Goal

Create an architecture diagram and implementation plan identifying the presentation, application, and database components, the chosen Azure services, the public entry point, and the internal traffic paths.

### Evidence

#### Screenshot 1 — Architecture diagram showing the public entry point, three tiers, network boundaries, and traffic flow

![alt text](<Week 07 Assignment 06_Screenshot 1.png>)

---

#### Screenshot 2 — Written architecture assumptions and selected Azure services

![alt text](<Week 07 Assignment 06_Screenshot 2.png>)
![alt text](<Week 07 Assignment 06_Screenshot 2.1png.png>)

---

# Task 2 — Create the Azure Network Foundation

## Goal

Create a dedicated Resource Group and VNet with separate subnets for the web, application, and database tiers, keeping the application and database tiers without direct public access.

### Evidence

#### Screenshot 3 — Resource Group overview showing the assignment resources

![alt text](<Week 07 Assignment 06_Screenshot 3.png>)

---

#### Screenshot 4 — VNet overview showing the address space and all required subnets

![alt text](<Week 07 Assignment 06_Screenshot 4.png>)

---

#### Screenshot 5 — Route-table or Private DNS evidence where applicable

![alt text](<Week 07 Assignment 06_Screenshot 5-1.png>)
![alt text](<Week 07 Assignment 06_Screenshot 5.1png.png>)

---

# Task 3 — Configure Security and Secret Management

## Goal

Apply least-privilege NSG rules so traffic flows Internet → public entry point → web tier → application tier → database tier, and store credentials in Azure Key Vault or another approved secure mechanism.

### Evidence

#### Screenshot 6 — NSG rules proving least-privilege access between the tiers

![alt text](<Week 07 Assignment 06_Screenshot 6.1.png>)
![alt text](<Week 07 Assignment 06_Screenshot 6.2.png>)
![alt text](<Week 07 Assignment 06_Screenshot 6.3.png>)

---

#### Screenshot 7 — Key Vault or approved secret-management configuration (without displaying secret values)

![alt text](<Week 07 Assignment 06_Screenshot 7.png>)

---

# Task 4 — Deploy the Presentation (Web) Tier

## Goal

Deploy the Book Review App presentation layer on the approved web-tier compute service, configured to route requests to the internal application-tier endpoint, and not directly exposed except through the public entry service.

### Evidence

#### Screenshot 8 — Web-tier compute overview showing subnet and availability configuration

![alt text](<Week 07 Assignment 06_Screenshot 8-1.png>)

---

#### Screenshot 9 — Terminal or service output proving the presentation layer is running

Add your screenshot here.

---

# Task 5 — Deploy the Business (Application) Tier

## Goal

Deploy the Book Review App backend privately in the application subnet, configured to use the private database endpoint and secured environment values, reachable only through its internal endpoint.

### Evidence

#### Screenshot 10 — Application-tier compute overview showing private subnet placement

![alt text](<Week 07 Assignment 06_Screenshot 10.png>)

---

#### Screenshot 11 — Backend process, service, or listening-port evidence

![alt text](<Week 07 Assignment 06_Screenshot 11.png>)

---

#### Screenshot 12 — Internal health-check or API response (without exposing secrets)

![alt text](<Week 07 Assignment 06_Screenshot 12.png>)

---

# Task 6 — Deploy the Managed Database Tier

## Goal

Create a private Azure managed database (public access disabled), with availability/backup/retention settings, the Book Review App schema imported, and access restricted to the application tier only.

### Evidence

#### Screenshot 13 — Database overview showing private connectivity and public access disabled

![alt text](<Week 07 Assignment 06_Screenshot 13.png>)

---

#### Screenshot 14 — Availability, backup, and retention configuration

![alt text](<Week 07 Assignment 06_Screenshot 14.png>)

---

#### Screenshot 15 — Successful schema or connectivity verification (without exposing credentials)

![alt text](<Week 07 Assignment 06_Screenshot 15.png>)

---

# Task 7 — Configure Traffic Management, Availability, and Monitoring

## Goal

Configure the approved public entry service with health probes and backend pools, internal routing for the application tier where required, and enable Azure Monitor/diagnostics/logs/alerts for the key resources.

### Evidence

#### Screenshot 16 — Public entry service showing listener, frontend endpoint, and healthy web targets

![alt text](<Week 07 Assignment 06_Screenshot 16.png>)

---

#### Screenshot 17 — Internal application-tier load-balancing or routing configuration where applicable

![alt text](<Week 07 Assignment 06_Screenshot 17.png>)

---

#### Screenshot 18 — Azure Monitor, diagnostic settings, logs, metrics, or alert evidence

![alt text](<Week 07 Assignment 06_Screenshot 18.png>)

---

# Task 8 — Validate the Production-Style Deployment

## Goal

Confirm the Book Review App works end to end through the public endpoint, with at least one database read and one write, confirm private tiers are not internet-reachable, and complete a safe availability test.

### Evidence

#### Screenshot 19 — Browser showing the Book Review App through the public endpoint

![alt text](<Week 07 Assignment 06_Screenshot 19.png>)

---

#### Screenshot 20 — Proof of successful database-backed read and write operations

![alt text](<Week 07 Assignment 06_Screenshot 20.png>)

---

#### Screenshot 21 — Evidence that private tiers are not publicly accessible

![alt text](<Week 07 Assignment 06_Screenshot 21.png>)

---

#### Screenshot 22 — Availability-test and healthy-target evidence

![alt text](<Week 07 Assignment 06_Screenshot 22.png>)

---

#### Public Endpoint

Paste your public endpoint URL here:

http://20.48.68.104

---

### Notes

Summarize what worked, issues encountered and how they were fixed, and the availability/security/secrets/monitoring/backup choices made.

### Notes

The Book Review App was successfully deployed using a three-tier architecture in Azure. The web tier is hosted on an Azure VM behind a Standard Public Load Balancer, the application tier runs as a private Azure Container Instance, and the database tier uses Azure Database for MySQL Flexible Server. The application was successfully accessed through the public Load Balancer endpoint, and end-to-end database connectivity was validated by displaying seeded book records and completing an **Add to Cart** operation.

Several issues were encountered and resolved during deployment. Initial SSH and network connectivity problems were corrected by reviewing the subnet and NSG configuration. A regional public-IP quota prevented creation of another public IP, so the existing Standard public IP was safely reassociated with the Load Balancer and used as the controlled public entry point. The application initially displayed **“no books available.”** Container logs and direct database queries confirmed that connectivity to MySQL was working but the `Book` table was empty. The application's supplied seed files were then used to populate **53 authors and 54 books**. A subsequent Add to Cart operation was verified by confirming a record in the `Cart` table.

Security was implemented using network segmentation and least-privilege access. The web, application, and database tiers use separate subnets and NSGs. Only the Load Balancer/web entry point is Internet-facing. The application container uses private IP `10.0.2.4` with no public FQDN, while MySQL is integrated with the delegated `db-subnet` and Private DNS. TLS/SSL is enforced for MySQL connections. Azure Key Vault was used for centralized secret-management configuration and RBAC permissions rather than embedding credentials in application source code. Any credential exposed during deployment/testing should be rotated after validation.

For availability and monitoring, the Standard Azure Load Balancer uses an HTTP health probe to monitor the web-tier backend. A controlled availability test was performed by temporarily stopping Nginx; the backend changed from healthy to unhealthy as expected. After Nginx was restarted, the target recovered to healthy status. Azure Load Balancer Insights/Monitor provides health and operational visibility. Database resilience is supported through Azure MySQL Flexible Server's managed backup and restore capabilities, while the deployment uses a single web VM, so the availability test demonstrates **failure detection and recovery rather than zero-downtime high availability**.


---

# Submission Instructions

- Add all required screenshots and links in your submission
- Do not expose passwords, keys, connection strings, or subscription IDs

---

# Completion Checklist

- [x] Task 1: Architecture diagram and assumptions documented (Screenshots 1–2)
- [x] Task 2: Network foundation created with isolated tiers (Screenshots 3–5)
- [x] Task 3: Least-privilege security and secret management configured (Screenshots 6–7)
- [x] Task 4: Presentation tier deployed (Screenshots 8–9)
- [x] Task 5: Application tier deployed privately (Screenshots 10–12)
- [x] Task 6: Managed database tier deployed privately (Screenshots 13–15)
- [x] Task 7: Public entry, internal routing, and monitoring configured (Screenshots 16–18)
- [x] Task 8: End-to-end validation and availability test completed (Screenshots 19–22, Public Endpoint, Notes)
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
