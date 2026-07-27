# Assignment 6 — Build an AI-Assisted Linux Health Check (AI-Assisted Linux Incident Triage)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will build a read-only Bash triage script that checks the health of your Ubuntu server and Nginx application, connect it to Claude Code as a reusable `/linux-triage` skill, simulate a controlled Nginx incident, use the skill to gather and analyze evidence, recover the service manually, and verify recovery. The workflow follows the Agentic Loop: Gather → Analyze → Human Act → Verify.

---

# Task 1 — Confirm the Healthy Baseline and Create the Workspace

## Goal

Confirm that Nginx and the React application are healthy before building the automation.

### Evidence

#### Screenshot 1 — Output of `systemctl is-active nginx`, `ss -ltn | grep ':80'`, and `curl -I http://localhost`

![alt text](<screenshots/Week 03_Assignment 06_Screenshot 1.png>)
![alt text](<screenshots/Week 03_Assignment 06_Screenshot 1.1png.png>)
---

#### Screenshot 2 — Output of `pwd` and `find . -maxdepth 4 -type d | sort` showing the workspace folder structure

![alt text](<screenshots/Week 03_Assignment 06_Screenshot 2.png>)

---

### Notes

Answer the following in your own words:

**1. What proves that Nginx is running?**

Nginx is proven to be running because the systemctl status nginx command shows that the service is active (running), and the Nginx logs confirm that the service started successfully without errors.

---

**2. What proves that the server is listening for HTTP traffic?**

The server is listening for HTTP traffic because Nginx is configured to listen on port 80, and a curl request to the server IP returns an HTTP 200 OK response, confirming that web traffic is being received and served.

---

**3. Why must you capture a healthy baseline before simulating an incident?**

A healthy baseline shows the normal state and performance of the server before a problem occurs. It helps with comparison during troubleshooting, making it easier to identify abnormal behavior, measure the impact of the incident, and confirm that recovery actions restore the system to its normal state.

---

# Task 2 — Create Project Context and Safety Rules in CLAUDE.md

## Goal

Tell Claude exactly what this project does and what it is not allowed to do.

### Evidence

#### Screenshot 3 — CLAUDE.md open in VS Code showing all four sections (Project Overview, Incident Workflow, Safety Rules, Output Rules)

![alt text](<screenshots/Week 03_Assignment 06_Screenshot 3.png>)

---

### Notes

Answer the following in your own words:

**1. Why should Claude receive project-specific operational rules?**

Project-specific operational rules provide Claude with the correct context, expectations, and safety guidelines for the environment. This helps Claude give more accurate suggestions, follow team practices, and avoid recommending actions that could negatively affect the system.

---

**2. Why is the human required to execute the recovery command?**

The human must execute recovery commands because they are responsible for approving changes that can affect production systems. This prevents accidental damage, ensures proper authorization, and keeps control of critical operations with the system owner.

---

**3. Which rule prevents Claude from making an unsupported diagnosis?**

The rule that requires Claude to avoid assumptions and only provide conclusions supported by available evidence prevents unsupported diagnosis. Claude should analyze logs, metrics, and system information before identifying the cause of an issue.


---

# Task 3 — Use Agentic AI to Plan Before Writing the Script

## Goal

Use Claude Code to inspect the environment and produce a read-only plan before creating any Bash code.

### Evidence

#### Screenshot 4 — Claude Code showing the five-check plan and read-only inspection results

![alt text](<screenshots/Week 03_Assignment 06_Screenshot 4.png>)

---

### Notes

Answer the following in your own words:

**1. Which part of this task represents the Gather phase?**

The Gather phase is when Claude collects and reviews the available information, such as the project files, configuration, and requirements, before suggesting a solution. This helps ensure the recommendations are based on the current environment.

---

Yes. Claude only analyzed the project and provided recommendations without creating or modifying any files. I verified this by checking that no new files appeared in the project and that git status showed no unexpected file changes.

---

**3. Why is planning before coding useful in DevOps automation?**

Planning before coding helps identify the requirements, potential risks, and the best approach before making changes. This reduces errors, prevents unnecessary work, and makes automation scripts more reliable and easier to maintain.

---

# Task 4 — Build the Linux Triage Bash Script

## Goal

Create one Bash script that gathers consistent Linux and Nginx health evidence.

### Evidence

#### Screenshot 5 — Top section of `linux-triage.sh` showing variables, thresholds, and the checks array

![alt text](<screenshots/Week 03_Assignment 06_Screenshot 5.png>)

---

#### Screenshot 6 — Middle section showing check functions and conditionals

![alt text](<screenshots/Week 03_Assignment 06_Screenshot 6.png>)

---

#### Screenshot 7 — Bottom section showing the loop, summary function, and exit behavior

![alt text](<screenshots/Week 03_Assignment 06_Screenshot 7.png>)

---

#### Screenshot 8 — Output of `bash -n scripts/linux-triage.sh` (no syntax errors) and `ls -l scripts/linux-triage.sh` showing executable permission

![alt text](<screenshots/Week 03_Assignment 06_Screenshot 8.png>)

---

### Notes

Answer the following in your own words:

**1. What is stored in the checks array?**

The checks array stores the names or commands of the different health checks that the script needs to run, such as checking CPU, memory, disk usage, or system information.

---

**2. How does the `for` loop use that array?**

The for loop goes through each item stored in the checks array one by one and runs the required health check for each item.

---

**3. Why are the health checks separated into functions?**

Separating health checks into functions makes the script easier to read, maintain, and troubleshoot. Each function handles one specific task, allowing changes or fixes to be made without affecting the entire script.

---

**4. What is the purpose of `$(...)` in this script?**

$(...) is used for command substitution in Bash. It runs a command and replaces it with the command's output, allowing the script to store or use the result in a variable or another command.

---

**5. Why does the script use different exit codes for HEALTHY, WARN, and FAIL?**

The script uses different exit codes to clearly communicate the health status of the system to users or other automation tools. A HEALTHY status indicates everything is working normally, WARN shows a potential issue that needs attention, and FAIL indicates a serious problem that requires action. These codes help monitoring systems and CI/CD pipelines automatically detect and respond to different conditions.

---

# Task 5 — Run and Understand the Healthy-State Report

## Goal

Run the Bash script against the healthy server and verify that it creates a report.

### Evidence

#### Screenshot 9 — Output of `./scripts/linux-triage.sh` showing your Full Name and all five check results

![alt text](<screenshots/Week 03_Assignment 06_Screenshot 9.png>)

---

#### Screenshot 10 — Output showing the captured exit code and final summary

![alt text](<screenshots/Week 03_Assignment 06_Screenshot 10.png>)

---

### Notes

Answer the following in your own words:

**1. What is the overall status of your healthy baseline?**

The overall status of the healthy baseline is HEALTHY because the server, Nginx service, application, and system resources were working normally without any detected issues.

---

The evidence is the successful HTTP response from the server using a command such as curl -I http://<server-ip>, which returned HTTP/1.1 200 OK. This confirms that Nginx is running and the application is responding to web requests..

---

**3. Did your script return exit code 0 or 1? Explain why.**

The script returned exit code 0 because all health checks passed successfully and the system was operating within the expected healthy conditions.

---

**4. What is the difference between a warning and a failure in this script?**

A warning means the system has a condition that requires attention but is still functioning, such as high resource usage. A failure means a critical issue has occurred, such as a stopped service or unavailable application, and immediate action is required

---

# Task 6 — Create and Run the /linux-triage Skill

## Goal

Turn the Bash script into a reusable, manually invoked Agentic AI workflow.

### Evidence

#### Screenshot 11 — `SKILL.md` showing the frontmatter, allowed tool restrictions, and safety rules

![alt text](<screenshots/Week 03_Assignment 06_Screenshot 11.png>)

---

#### Screenshot 12 — `/linux-triage` output for the healthy server

![alt text](<screenshots/Week 03_Assignment 06_Screenshot 12.png>)

---

### Notes

Answer the following in your own words:

**1. Why does this skill have Bash, Read, and Grep, but not Write?**

The skill includes Bash, Read, and Grep because it only needs to collect and analyze system information, logs, and configuration files. It does not include Write because the skill should not modify files or make changes to the server, which reduces the risk of accidental damage.

---

**2. Why is `disable-model-invocation: true` useful for this skill?**

disable-model-invocation: true prevents Claude from automatically running the skill without the user requesting it. This gives the human operator control and ensures that diagnostic actions only happen when intentionally triggered.

---

Bash performs the actual Linux commands, such as checking processes, services, disk usage, and logs. Claude analyzes the collected information, explains the findings, and helps identify possible issues or next steps.

---

**4. Why is this better than asking Claude "Is my server healthy?" without giving it evidence?**

This approach is better because Claude makes decisions based on real system evidence instead of guessing. Providing command outputs, logs, and metrics allows Claude to give more accurate troubleshooting advice and reduces unsupported conclusions.

---

# Task 7 — Simulate an Nginx Incident and Let the Skill Diagnose It

## Goal

Create a controlled service failure, gather evidence through Bash, and let Claude analyze the evidence without taking recovery action.

### Evidence

#### Screenshot 13 — Output showing Nginx is inactive and the HTTP request fails

![alt text](<screenshots/Week 03_Assignment 06_Screenshot 13.png>)

---

#### Screenshot 14 — `/linux-triage` output showing failed evidence, most likely cause, and a suggested recovery command

![alt text](<screenshots/Week 03_Assignment 06_Screenshot 14.png>)

---

#### Screenshot 15 — `incident-failure-report.txt` showing the failed checks and your Full Name

![alt text](<screenshots/Week 03_Assignment 06_Screenshot 15.png>)
---

### Notes

Answer the following in your own words:

**1. Which three checks failed?**

The three failed checks were the Nginx service check, the HTTP availability check, and the application response check. These failures showed that the web service was not available or not responding correctly.

---

**2. What evidence supports the conclusion that Nginx is unavailable?**

The evidence was the failed service status check showing that Nginx was not running properly, along with unsuccessful HTTP requests that did not return the expected HTTP 200 OK response.

---

**3. Did Claude execute the recovery command? Why is that important?**

No, Claude did not execute the recovery command. This is important because system recovery actions can affect production services, so a human must review and approve changes before they are executed.

---

**4. Which phase of the Agentic Loop is represented by the Bash report?**

The Bash report represents the Gather phase because it collects system information, logs, and health check results needed for analysis.

---

**5. Which phase is represented by Claude's explanation?**

Claude's explanation represents the Reason phase because it analyzes the gathered evidence, identifies possible causes, and provides recommendations based on the available information.

---

# Task 8 — Recover Manually, Verify Again, and Write the Incident Summary

## Goal

Recover the service as the human operator and prove that the system is healthy again.

### Evidence

#### Screenshot 16 — Output showing Nginx is active and `curl -I http://localhost` returns 200 OK

![alt text](<screenshots/Week 03_Assignment 06_Screenshot 16.png>)

---

#### Screenshot 17 — Second `/linux-triage` output showing successful recovery with no FAIL results

![alt text](<screenshots/Week 03_Assignment 06_Screenshot 17.png>)

---

#### Screenshot 18 — Output of `ls -lah reports` showing both `incident-failure-report.txt` and `recovery-report.txt`

![alt text](<screenshots/Week 03_Assignment 06_Screenshot 18.png>)

---

#### Screenshot 19 — `incident-summary.md` showing all required sections and your Full Name

![alt text](<screenshots/Week 03_Assignment 06_Screenshot 19.png>)

---

### Notes

Answer the following in your own words:

**1. What action did you execute manually?**

I manually executed the recovery command to restart the Nginx service: sudo systemctl restart nginx

---

**2. What evidence proves that the service recovered?**

The service recovery was confirmed because Nginx started successfully, and a curl request returned an HTTP 200 OK response, proving that the server was responding and the application was available.

---

**3. Why is the second triage run necessary?**

The second triage run is necessary to verify that the recovery action worked and that the system returned to a healthy state. It confirms that the original problem was resolved instead of assuming the fix was successful.

---

**4. What could go wrong if an AI agent automatically restarted every failed service?**

An AI agent restarting every failed service could cause more problems by restarting critical services unnecessarily, hiding underlying issues, causing downtime, or making unwanted changes without proper human approval.

---

**5. In one sentence, explain the difference between using AI as a chatbot and using AI in this agentic workflow.**

A chatbot only provides answers based on questions, while an agentic workflow uses AI to gather evidence, analyze situations, recommend actions, and work with human approval to safely resolve problems.

---

# Incident Summary

Fill in all seven sections below in your own words.

**Full Name:** <Osamudiamen Agharese>  

**Date:** <19/07/2026>

---

**1. Reported Symptom**

The application appeared to be unavailable because the website was not responding correctly to HTTP requests. Users could not access the application as expected.

---

**2. Evidence Collected**

The Bash health checks showed failures related to the web service:
- Nginx service check indicated that Nginx was not running correctly.
- HTTP availability check failed because the server did not return the expected successful response.
- Application availability check confirmed that the application was not serving traffic.

---

**3. Most Likely Cause**

Based on the collected evidence, the most likely cause was that the Nginx service or web content configuration was unavailable, preventing the application from responding to HTTP requests.

---

**4. Human-Approved Recovery Action**

After reviewing the evidence, the recovery command was manually executed: 
sudo systemctl start nginx
systemctl is-active nginx
curl -I http://localhost
---

**5. Verification**

Recovery was confirmed by:
Checking that Nginx restarted successfully.
Running a curl request that returned:
HTTP/1.1 200 OK

---

**6. Safety Decision**

The AI skill was allowed to gather and analyze evidence because it only needed read-only access to logs, system information, and service status. It was not allowed to restart the service because recovery actions can affect system availability and require human approval.

---

**7. Agentic Loop Mapping**

The incident followed the Agentic Loop:
Gather: Bash commands collected system health information, service status, and application availability evidence.
Reason: Claude analyzed the collected evidence and identified the likely cause.
Act: The human reviewed and manually executed the approved recovery command.
Verify: The system was checked again to confirm Nginx and the application had recovered.

---

# LinkedIn Post (Required)

## Evidence

#### LinkedIn Post URL

`https://www.linkedin.com/posts/ossa-agharese-01991a233_devops-agenticai-bashscripting-share-7484753107260997632-Va2E/?utm_source=share&utm_medium=member_desktop&rcm=ACoAADpVsJUBYBjWiWLSBz1ZojH27wf_yizZYUA`

---

#### Screenshot — Published LinkedIn post

![alt text](<screenshots/LinkedIn Post_4.png>)

---

# GitHub Repository URL

`https://github.com/23110100/devops-micro-internship-pravinmishra.git`

---

# Submission Instructions

- Add all required screenshots in your submission
- Full Name must be visible in required screenshots and the Bash report
- All written answers must be in your own words
- Do not expose sensitive information (keys, passwords, AWS account IDs, tokens)
- GitHub URL must be included in this document

---

# Completion Checklist

- [x] Task 1: Healthy baseline confirmed, workspace created (Screenshots 1–2, Notes answered)
- [x] Task 2: CLAUDE.md created with all four sections (Screenshot 3, Notes answered)
- [x] Task 3: Five-check plan produced by Claude using read-only tools (Screenshot 4, Notes answered)
- [x] Task 4: `linux-triage.sh` created, syntax validated, executable permission set (Screenshots 5–8, Notes answered)
- [x] Task 5: Healthy-state report generated with no FAIL result (Screenshots 9–10, Notes answered)
- [x] Task 6: `/linux-triage` skill created and run successfully on healthy server (Screenshots 11–12, Notes answered)
- [x] Task 7: Nginx incident simulated, failed evidence captured, Claude did not execute recovery (Screenshots 13–15, Notes answered)
- [x] Task 8: Nginx recovered manually, recovery verified, reports saved, incident summary complete (Screenshots 16–19, Notes answered)
- [x] Incident summary contains all seven required sections
- [x] LinkedIn post published and URL submitted
- [x] Full Name visible in all required screenshots and the Bash report
- [x] Skill does not have Write permission
- [x] Skill did not execute any recovery commands
- [x] No sensitive data exposed

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://pravinmishra.com/dmi  
- 🎓 DevOps for Beginners (Udemy): https://www.udemy.com/course/devops-for-beginners-docker-k8s-cloud-cicd-4-projects/  
- 🎓 Agentic AI DevOps with Claude Code: https://www.udemy.com/course/ultimate-agentic-ai-devops-with-claude-code/  
- 🎓 DevOps with Claude Code: Terraform, EKS, ArgoCD & Helm: https://www.udemy.com/course/devops-with-claude-code-terraform-eks-argocd-helm/  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*