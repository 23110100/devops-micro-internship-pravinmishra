# Assignment 3 — Production Maintenance Drill (OPS Checklist)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will treat your already deployed React application (on Ubuntu VM with Nginx) as a live production system. You will perform structured operational checks covering network validation, service health, log analysis, resource monitoring, configuration verification, and incident simulation with recovery — mirroring real on-call DevOps responsibilities.

---

# Task 1 — Server Access & Networking Validation

## Goal

Verify that the deployed React application is reachable from the browser and confirm basic network connectivity of the Ubuntu VM.

### Evidence

#### Screenshot 1 — Browser showing the React app with your Full Name visible on the UI

![alt text](<screenshots/Week 03_Assignment 03_Screenshot 1.png>)

---

#### Screenshot 2 — Output of `ip a`

![alt text](<screenshots/Week 03_Assignment 03_Screenshot 2.png>)

---

#### Screenshot 3 — Output of `sudo ss -tulpen`

![alt text](<screenshots/Week 03_Assignment 03_Screenshot 3.png>)

---

#### Screenshot 4 — Output of `sudo ufw status`

![alt text](<screenshots/Week 03_Assignment 03_Screenshot 4.png>)

---

### Notes

Answer the following in your own words:

**1. What proves Nginx is listening on 0.0.0.0:80?**

The `ss -tulnp` command showed Nginx listening on `0.0.0.0:80`, which means it is accepting HTTP connections on port 80 from any network interface on the server.

---

**2. What proves SSH is active on port 22?**

The `ss -tulnp` command also showed the `sshd` service listening on port `22`, confirming that the SSH service is running and available for remote connections.

---

**3. Did you find any unexpected open ports? Explain briefly.**

No. Only the expected ports were open: port **22** for SSH and port **80** for Nginx. No unexpected ports were found, indicating the server is exposing only the required services.

---

# Task 2 — Service Health & Systemd Validation (Nginx)

## Goal

Verify that Nginx is properly installed, running, enabled at boot, and safely configured.

### Evidence

#### Screenshot 1 — Output of `systemctl status nginx --no-pager`

![alt text](<screenshots/Week 03_Assignment 03_Screenshot 5.png>)

---

#### Screenshot 2 — Output of `sudo nginx -t`

![alt text](<screenshots/Week 03_Assignment 03_Screenshot 6.png>)

---

#### Screenshot 3 — Output of `sudo ss -lptn '( sport = :80 )'`

![alt text](<screenshots/Week 03_Assignment 03_Screenshot 7.png>)

---

### Notes

Answer the following in your own words:

**1. What happens if Nginx fails to restart in production?**

If Nginx fails to restart in production, the website or application being served through Nginx may become unavailable. Users could receive connection errors because the server is no longer accepting HTTP requests. The first step would be to check the Nginx error logs and service status to identify the cause of the failure and fix the issue quickly.

---

My basic rollback plan is to restore the previous working configuration or application version. Before making changes, I would keep backups of the Nginx configuration files and the current deployment. If the new change causes problems, I would revert to the previous configuration, restart Nginx, and verify that the service is running correctly.

---

# Task 3 — Logs & Request Trace

## Goal

Verify real traffic flow and analyze logs to understand system behavior and errors.

### Evidence

#### Screenshot 1 — Output of `sudo tail -n 30 /var/log/nginx/access.log`

![alt text](<screenshots/Week 03_Assignment 03_Screenshot 8.png>)

---

#### Screenshot 2 — Output of `sudo tail -n 30 /var/log/nginx/error.log`

![alt text](<screenshots/Week 03_Assignment 03_Screenshot 9.png>)

---

#### Screenshot 3 — Output of `sudo journalctl -u nginx --no-pager -n 50`

![alt text](<screenshots/Week 03_Assignment 03_Screenshot 10.png>)

---

### Notes

Answer the following in your own words:

**1. Were there any errors in the logs?**

- If yes, mention 1–2 example error lines from the logs and explain what each one means in simple terms.
- If no, explain what it means if the error log is empty or shows no recent errors during your check.

No. The Nginx error log did not show any recent error messages. It only showed a normal notice message: `using inherited sockets from "5;6;"`, which is related to Nginx restarting and reusing existing connections. This is not an error. The absence of error messages means Nginx did not encounter any critical problems during the check.

---

**2. If there were no errors, what does that indicate about the system?**

It indicates that Nginx is running normally and there are no detected configuration, startup, or service issues. The web server is healthy and able to handle requests successfully.

---

**3. Based on the access logs, were your curl requests visible in the log entries? What does that prove about traffic flow?**

Yes, the curl requests were visible in the Nginx access logs. This confirms that requests from the client reached the server, Nginx received and processed the HTTP traffic, and the web server was successfully responding to incoming connections.


---

# Task 4 — System Resource Health Check (Capacity Red Flags)

## Goal

Assess server capacity and detect potential performance or failure risks.

### Evidence

#### Screenshot 1 — Output of `uptime`

![alt text](<screenshots/Week 03_Assignment 03_Screenshot 11.png>)

---

#### Screenshot 2 — Output of `free -h`

![alt text](<screenshots/Week 03_Assignment 03_Screenshot 12.png>)

---

#### Screenshot 3 — Output of `df -h`

![alt text](<screenshots/Week 03_Assignment 03_Screenshot 13-1.png>)

---

#### Screenshot 4 — Output of `sudo du -sh /var/* | sort -h`

![alt text](<screenshots/Week 03_Assignment 03_Screenshot 13.png>)

---

### Notes

Answer the following in your own words:

**1. Which resource looks most critical right now? (CPU/load, memory, or disk) Explain why.**

Memory appears to be the most critical resource because if available memory becomes too low, the server may slow down or start using swap space, which reduces performance. Monitoring memory usage helps prevent application slowdowns and unexpected crashes.

---

**2. What happens if disk becomes 100% full in a production server?**

If the disk becomes 100% full, the server may be unable to write logs, save files, or create temporary data. Applications can fail, databases may stop working correctly, and users may experience service outages. Monitoring disk usage and cleaning up unnecessary files helps prevent this problem.

---

# Task 5 — Configuration & Deployment Verification

## Goal

Ensure the correct React build is deployed and Nginx is serving it properly.

### Evidence

#### Screenshot 1 — Output of `ls -lah /var/www/html | head -n 20`

![alt text](<screenshots/Week 03_Assignment 03_Screenshot 14.png>)

---

#### Screenshot 2 — Output of `grep -R "Deployed by" -n /var/www/html 2>/dev/null | head`

![alt text](<screenshots/Week 03_Assignment 03_Screenshot 15.png>)

---

#### Screenshot 3 — Output of `grep -n "try_files" /etc/nginx/sites-available/default`

![alt text](<screenshots/Week 03_Assignment 03_Screenshot 16.png>)

---

### Notes

Answer the following in your own words:

**1. How do you confirm that the correct version of the application is deployed?**


I confirmed when I ran the grep -R "Deployed by" -n /var/www/html 2>/dev/null | head command, searched to changes made-my name. Also  I confirmed the correct version is deployed by opening the application in a web browser or using `curl` to verify it loads successfully and displays the expected content. I also compare the deployed files with the latest build and check the deployment logs to ensure the correct version was copied to the web server without errors.


---

# Task 6 — Nginx Configuration Failure Simulation

## Goal

Simulate a real-world Nginx misconfiguration and recover the service safely.

### Evidence

#### Screenshot 1 — Output of `sudo nginx -t` showing the syntax error (broken config)

![alt text](<screenshots/Week 03_Assignment 03_Screenshot 17-3.png>)

---

#### Screenshot 2 — Output of `sudo nginx -t` showing syntax ok (fixed config)

![alt text](<screenshots/Week 03_Assignment 03_Screenshot 17-1.png>)

---

#### Screenshot 3 — Output of `curl -I http://<public-ip>` confirming recovery (200 OK)

![alt text](<screenshots/Week 03_Assignment 03_Screenshot 18-1.png>)

---

### Notes

Answer the following in your own words:

**1. What caused the configuration failure?**

The configuration failure was caused by an error in the Nginx configuration file. This could be a missing bracket, incorrect syntax, or an invalid directive, which prevented Nginx from validating and loading the configuration.

---

**2. How did you fix the issue?**

I reviewed the Nginx configuration file, corrected the syntax error, and tested the configuration using sudo nginx -t. After confirming the configuration was valid, I restarted the Nginx service with sudo systemctl restart nginx.

---

**3. How can you avoid this kind of issue in real production systems?**

To avoid this issue, always validate configuration changes with sudo nginx -t before restarting Nginx. It's also important to back up configuration files, use version control, review changes carefully, and test updates in a staging environment before deploying them to production.

---

# Task 7 — Web Application Failure Simulation

## Goal

Simulate missing deployment content and recover the application safely.

### Evidence

#### Screenshot 1 — Output of `curl -I http://<public-ip>` showing failure (non-200 response)

![alt text](<screenshots/Week 03_Assignment 03_Screenshot 19.png>)

---

#### Screenshot 2 — Output of `curl -I http://<public-ip>` confirming recovery (200 OK)

![alt text](<screenshots/Week 03_Assignment 03_Screenshot 20.png>)

---

### Notes

Answer the following in your own words:

**1. What caused the application to break in this scenario?**

The application broke because the web root directory /var/www/html used by Nginx was temporarily moved and replaced with an empty directory. Since the application files were no longer available in the expected location, Nginx could not serve the website correctly, resulting in an HTTP 500 Internal Server Error.

---

**2. How did you fix the issue and restore the application?**

I restored the application by removing the empty /var/www/html directory and moving the backup directory (/var/www/html_backup) back to its original location. I then restarted Nginx and verified the recovery using curl -I, which returned HTTP 200 OK, confirming that the application was working again.

---

**3. What steps would you take to prevent this kind of issue in real production systems?**

To prevent this issue, I would create backups before making changes, test configuration changes before deployment, and use version control for application releases. I would also use deployment strategies such as blue-green deployment or rolling updates, perform health checks after deployment, and maintain a rollback plan to quickly restore the previous working version if a problem occurs.

---

# Task 8 — Security & Reliability Review

## Goal

Review and reflect on the security and reliability practices applied during this assignment.

### Security & Reliability Notes

Answer the following in your own words:

**1. Why is SSH key-based authentication more secure than sharing passwords?**

SSH key-based authentication is more secure because it uses a cryptographic key pair instead of a password that can be guessed, stolen, or reused. The private key stays with the user, while the server only stores the public key, reducing the risk of unauthorized access.

---

**2. Why should only required ports be open on a production server?**

Only necessary ports should be open to reduce the attack surface. Each open port is a possible entry point for attackers, so limiting access to required services improves server security.

---

**3. Why is it important for Nginx to be enabled on boot?**

Enabling Nginx on boot ensures that the web server automatically starts after a server restart or system failure. This helps maintain application availability without requiring manual intervention.

---

**4. What are the risks of sharing secrets, keys, or credentials publicly?**

Sharing secrets or credentials publicly can allow unauthorized users to access systems, steal data, modify applications, or create unexpected costs. Sensitive information such as SSH keys, API keys, and passwords should always be protected.

---

**5. Why should cloud resources be stopped or terminated when they are no longer needed?**

Unused cloud resources should be stopped or terminated to avoid unnecessary costs, reduce security risks, and prevent unused services from becoming potential targets for attackers. Proper resource management helps maintain a secure and cost-effective cloud environment.

---

# LinkedIn Post (Required)

## Evidence

#### LinkedIn Post URL

`https://www.linkedin.com/posts/ossa-agharese-01991a233_devops-systemsengineering-nginx-share-7484745054167703552-r7CG/?utm_source=share&utm_medium=member_desktop&rcm=ACoAADpVsJUBYBjWiWLSBz1ZojH27wf_yizZYUA`


---

#### Screenshot — Published LinkedIn post

![alt text](<screenshots/LinkedIn Post_2.png>)

---

# Submission Instructions

- Add all required screenshots in your submission
- Full name must be visible in required screenshots
- Do not expose sensitive information (keys, passwords, account IDs)

---

# Completion Checklist

- [x] Task 1: Screenshots (browser, ip a, ss -tulpen, ufw status) + Notes answered
- [x] Task 2: Screenshots (nginx status, nginx -t, ss port 80) + Notes answered
- [x] Task 3: Screenshots (access log, error log, journalctl) + Notes answered
- [x] Task 4: Screenshots (uptime, free -h, df -h, du -sh) + Notes answered
- [x] Task 5: Screenshots (ls html, grep deployed by, grep try_files) + Notes answered
- [x] Task 6: Screenshots (nginx -t fail, nginx -t pass, curl recovery) + Notes answered
- [x] Task 7: Screenshots (curl failure, curl recovery) + Notes answered
- [x] Task 8: Security & Reliability Notes answered
- [x] LinkedIn post published and URL submitted
- [x] Full Name visible in all required screenshots
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