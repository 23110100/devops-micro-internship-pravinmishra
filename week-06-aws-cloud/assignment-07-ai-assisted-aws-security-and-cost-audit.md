# Assignment 7 — AI-Assisted AWS Security and Cost Audit

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will build a read-only Bash script that audits the AWS resources you deployed earlier this week — your S3 static site, EC2 instance(s), security groups, RDS database, and EBS volumes — for common security and cost misconfigurations.

You will then connect that script to Claude Code as a reusable `/aws-audit` skill that explains what it found and recommends a fix, without ever making the fix itself.

Finally, you will find a real misconfiguration in your own account, apply the fix yourself, and prove it worked with a second audit run.

---

# Task 1 — Confirm Your AWS Resources and Set Up Your Workspace

## Goal

Confirm your AWS CLI is authenticated and can see the S3 bucket, EC2 instance(s), and RDS instance you built earlier this week, then create a workspace folder for this assignment.

### Evidence

#### Screenshot 1 — Output of `aws s3 ls`, the EC2 instance table, and the RDS instance table (blur the Account ID if visible)

![alt text](<Week 06 Assignment 07_Screenshot 1.png>)

---

#### Screenshot 2 — Output of `pwd` and `find . -maxdepth 4 -type d | sort`

![alt text](<Week 06 Assignment 07_Screenshot 2.png>)

---

### Notes You Must Write (Very Important)

**1. Which resources from this week's earlier assignments did you see in the listings?**

I confirmed that the AWS CLI could see resources from my earlier assignments. These included my S3 bucket sammy-dmi-portfolio-2026, multiple EC2 instances including epicbook-ec2 and book-review-app-ec2, and RDS MySQL databases including database-1, epicbook-db, and ha-db. The EC2 and RDS resources were running or available in the us-east-2 region.

**2. Why must you confirm your resources exist before writing an audit script against them?**

I must confirm the resources exist and that my AWS CLI can access them before writing the audit script because the script depends on querying real AWS resources. This verifies that my authentication, permissions, account, and region are correct. It also establishes a known baseline so that if the audit script later returns missing or unexpected results, I can distinguish a script problem from an AWS authentication, permissions, region, or resource-availability problem.

---

# Task 2 — Define Safety Rules in CLAUDE.md

## Goal

Create a `CLAUDE.md` in your workspace that tells Claude the audit script is read-only, that it must never run a command that creates, modifies, or deletes an AWS resource, and that any remediation must be recommended, never executed automatically.

### Evidence

#### Screenshot 3 — `CLAUDE.md` open in VS Code showing all four sections

![alt text](<Week 06 Assignment 07_Screenshot 3.png>)

---

### Notes You Must Write (Very Important)

**1. Why should Claude never be given permission to run `revoke-security-group-ingress` itself, even if the fix is obviously correct?**

Claude should never be allowed to run revoke-security-group-ingress automatically because it is a write operation that changes live AWS infrastructure. Even when a security rule appears unsafe, automatically removing it could block legitimate application traffic, administrative access, or dependencies that were not visible during the audit. The audit should remain read-only: Claude can identify the risk and recommend the exact remediation, but a human must review and intentionally apply the change.

**2. Which rule prevents Claude from claiming a finding that the report does not support?**

The rule should be that Claude must base every finding and recommendation only on evidence contained in the audit report and must not invent, assume, or infer unsupported findings. If the report does not provide enough evidence to establish a problem, Claude should state that the finding cannot be confirmed rather than presenting it as a fact.

One note: that second rule is not explicitly stated in the four-section CLAUDE.md we created earlier. Since your assignment specifically asks about it, we should add an evidence-grounding rule to CLAUDE.md before you take Screenshot 3.

---

# Task 3 — Plan the Audit with Claude Code

## Goal

Ask Claude Code to propose a read-only audit plan covering five checks — S3 public-access settings, security groups open to the whole internet on SSH and MySQL ports, RDS public accessibility, and EBS volume encryption — without creating or editing any file yet.

### Evidence

#### Screenshot 4 — Claude Code showing the five-check plan

![alt text](<Week 06 Assignment 07_Screenshot 4.png>)

---

### Notes You Must Write (Very Important)

**1. Which part of this task represents the Gather phase?**

The Gather phase is the part where the audit plan identifies which AWS resources and configuration data need to be collected using read-only AWS CLI commands. This includes gathering S3 public-access settings, security group ingress rules, RDS public-accessibility settings, and EBS encryption status. At this stage, the goal is to collect evidence about the current AWS environment without changing any resources.

**2. Did every proposed command start with `describe-`, `get-`, or `list-`? Why does that matter?**

Yes, the proposed AWS CLI commands should use read-only operations beginning with describe-, get-, or list-. This matters because these commands retrieve information without modifying AWS infrastructure. Keeping the audit commands read-only reduces the risk of accidentally creating, changing, or deleting resources and supports the safety requirements defined in CLAUDE.md. Any remediation discovered by the audit should be recommended for manual review rather than executed automatically.

---

# Task 4 — Build the AWS Audit Script

## Goal

Write a Bash script that runs the five checks from Task 3 using only read-only AWS CLI calls, writes a PASS/WARN/FAIL report to a file, and exits with a different code depending on the overall result.

Make it executable and confirm it has no syntax errors.

### Evidence

#### Screenshot 5 — Top section of `aws-audit.sh` showing the variables and the checks array

![alt text](<Week 06 Assignment 07_Screenshot 5.png>)

---

#### Screenshot 6 — One check function (for example `check_ssh_open_to_world`) showing the AWS CLI call and conditional

![alt text](<Week 06 Assignment 07_Screenshot 6.png>)

---

#### Screenshot 7 — Output of `bash -n scripts/aws-audit.sh` and `ls -l scripts/aws-audit.sh`

![alt text](<Week 06 Assignment 07_Screenshot 7.png>)

---

### Notes You Must Write (Very Important)

**1. What is stored in the checks array, and how does the loop use it?**

The CHECKS array stores the names of the five audit checks: S3 public access, security groups open on SSH, security groups open on MySQL, RDS public accessibility, and EBS encryption. The array provides a centralized list of the audit categories so the script can reference or iterate through the checks consistently instead of hard-coding the names repeatedly. In the current script, the checks themselves are executed by their corresponding logic/functions, while the array documents the complete set of checks the audit is expected to perform.

**2. Why does every AWS CLI call in this script use `--query` and `--output text` instead of parsing raw JSON?**

Using --query lets the AWS CLI filter the response down to only the fields the audit actually needs, and --output text produces simple shell-friendly output. This makes the Bash conditions easier to read and reduces the need for extra JSON-processing tools such as jq. It also keeps the report concise and makes it easier to test whether a result is empty or contains a finding.

**3. Why does the script use different exit codes for HEALTHY, WARN, and FAIL?**

Different exit codes allow other tools, CI/CD pipelines, or automation systems to understand the overall audit result without having to parse the report text. An exit code of 0 represents a healthy result with no findings, 1 represents warnings that should be reviewed, and 2 represents more serious failures that require attention. This makes the script useful both for humans and for future automated monitoring while still keeping the audit itself read-only.

---

# Task 5 — Run the Baseline Audit

## Goal

Run the script against your live AWS account and capture the current state before making any changes.

### Evidence

#### Screenshot 8 — Output of `./scripts/aws-audit.sh` showing your Full Name and all five checks

![alt text](<Week 06 Assignment 07_Screenshot 8.png>)

---

#### Screenshot 9 — Output showing the captured exit code and final summary

![alt text](<Week 06 Assignment 07_Screenshot 9.png>)

---

### Notes You Must Write (Very Important)

**1. What is the overall status of your baseline audit?**

The overall status of my baseline audit is FAIL, with an exit code of 2. The audit completed all five checks and reported 2 PASS, 2 WARN, and 1 FAIL. This indicates that my AWS environment contains at least one security issue requiring attention, along with additional warning-level findings that should be reviewed.

**2. Did any check return FAIL or WARN? If so, which one, and what evidence did it show?**

Yes. The SSH security group check returned FAIL because four security groups allowed inbound SSH traffic on TCP port 22 from 0.0.0.0/0, meaning SSH was exposed to the entire internet. The audit identified launch-wizard-1, launch-wizard-2, launch-wizard-3, and launch-wizard-4.

Two checks also returned WARN. The S3 check reported that sammy-dmi-portfolio-2026 did not have all four S3 public-access-block settings enabled. The EBS encryption check identified eight in-use 8 GiB EBS volumes that were not encrypted. The MySQL security-group and RDS public-accessibility checks both passed.

**3. If every check passed, what does that tell you about the security posture of your account so far?**

If every check passed, it would indicate that the AWS resources examined by these five specific audit checks currently meet the security conditions defined in the script: S3 public-access blocking is enabled, SSH and MySQL are not exposed to the entire internet, RDS instances are not publicly accessible, and EBS volumes are encrypted.

However, a completely passing audit would not prove that the entire AWS account is secure. It would only show that no issues were detected within the scope of these five checks. A broader security assessment would still be required to evaluate areas such as IAM permissions, logging, encryption policies, patching, network architecture, backups, and other AWS services.

---

# Task 6 — Build and Run the /aws-audit Skill

## Goal

Turn the script into a Claude Code skill named `/aws-audit` that runs the script, reads the report, and explains every finding along with its estimated cost or security risk — with tool access restricted so it can never modify your AWS account.

### Evidence

#### Screenshot 10 — `SKILL.md` showing the frontmatter, tool restrictions, and safety rules

![alt text](<Week 06 Assignment 07_Screenshot 10.png>)

---

#### Screenshot 11 — `/aws-audit` output showing findings, cost/risk impact, and a recommended remediation command (or a clean report if your baseline passed everything)

![alt text](<Week 06 Assignment 07_Screenshot 11.png>)

---

### Notes You Must Write (Very Important)

**1. Why does this skill have Bash, Read, and Grep, but not Write?**

The skill needs Bash to execute the read-only aws-audit.sh script, Read to inspect the generated audit report, and Grep to search or filter relevant findings. It does not need Write because the purpose of the skill is to audit and explain the existing AWS environment, not modify files or infrastructure. Excluding Write follows the principle of least privilege and reduces the risk of Claude changing the audit script, report, or other project files while performing an audit.

**2. What part is performed by Bash, and what part is performed by Claude?**

Bash performs the deterministic data-gathering portion of the audit. The Bash script executes read-only AWS CLI commands, evaluates the five predefined checks, assigns PASS/WARN/FAIL results, writes the report, and produces the appropriate exit code. Claude then reads those results and performs the interpretation layer: it explains what each finding means, identifies the security or cost impact, and recommends an appropriate manual remediation. Claude does not execute the remediation.

**3. Why is estimating cost/risk impact something the AI adds on top of a plain PASS/FAIL script?**

A PASS/FAIL Bash script is effective at detecting predefined technical conditions, but the result alone does not explain their significance. AI can add context by interpreting the evidence and explaining the potential security or cost impact. For example, the script can detect that SSH port 22 is exposed to 0.0.0.0/0, while Claude can explain that this increases exposure to internet-based scanning and brute-force attempts and recommend restricting access. This combination keeps the actual audit deterministic and read-only while using AI to make the findings easier to understand and act upon.

---

# Task 7 — Fix a Real Finding and Re-Verify

## Goal

Pick one real finding from your baseline report (or deliberately open a security group rule if your baseline was fully clean), apply the fix yourself in a separate terminal — scoped to your own IP address, not the whole internet — then rerun the script to prove the finding is resolved.

### Evidence

#### Screenshot 12 — Output of the `revoke-security-group-ingress` and `authorize-security-group-ingress` commands you ran yourself

![alt text](<Week 06 Assignment 07_Screenshot 12.png>)

---

#### Screenshot 13 — Rerun of `./scripts/aws-audit.sh` showing the finding is now PASS

![alt text](<Week 06 Assignment 07_Screenshot 13.png>)

---

### Notes You Must Write (Very Important)

**1. Which exact finding did you fix, and what command did you run?**

I fixed the FAIL finding for security groups allowing SSH on TCP port 22 from 0.0.0.0/0. The baseline audit identified four affected launch-wizard security groups. I manually removed their world-open SSH rules using aws ec2 revoke-security-group-ingress and replaced them with SSH rules restricted to my current public IP using aws ec2 authorize-security-group-ingress.

For example, one remediation was:

aws ec2 revoke-security-group-ingress \
  --group-id sg-055dd893ccbad49b1 \
  --protocol tcp \
  --port 22 \
  --cidr 0.0.0.0/0

aws ec2 authorize-security-group-ingress \
  --group-id sg-055dd893ccbad49b1 \
  --protocol tcp \
  --port 22 \
  --cidr 104.205.5.165/32

After correcting all four security groups identified by the audit, the SSH check changed from FAIL to PASS.

**2. Why did you scope the new rule to your own IP address instead of leaving it open to `0.0.0.0/0`?**

I restricted SSH to my public IP using a /32 CIDR because 0.0.0.0/0 allows any internet host to attempt an SSH connection. Restricting port 22 to 104.205.5.165/32 follows the principle of least privilege by allowing administrative access only from my current public IP. This significantly reduces unnecessary exposure to internet scanning, brute-force attempts, and unauthorized SSH access.

**3. Did Claude execute the remediation command, or did you? Why does that matter?**

I executed the remediation commands myself in a separate Git Bash terminal. Claude did not make any changes to my AWS resources. Claude's role was limited to analyzing the read-only audit results and recommending remediation.

This separation matters because it keeps the AI-assisted audit read-only and ensures that infrastructure changes require deliberate human review and approval. It reduces the risk of an AI agent automatically making an incorrect or disruptive change to live AWS resources.

**4. Which phase of the Agentic Loop does the Bash script represent? Which phase does Claude's explanation represent? Which phase is you running the fix?**

The Bash audit script represents the Gather phase because it collects evidence about the current AWS environment using read-only AWS CLI commands. Claude's explanation represents the Reason phase, where the collected evidence is interpreted to determine the security or cost impact and recommend remediation. My manually running the AWS CLI remediation commands represents the Act phase, because that is when the approved infrastructure change is actually performed.

The second execution of aws-audit.sh then returns to the Gather phase, providing new evidence that the remediation worked. In this case, the SSH check changed from FAIL to PASS, while the overall audit improved from 2 PASS / 2 WARN / 1 FAIL to 3 PASS / 2 WARN / 0 FAIL.

---

# LinkedIn Post (Required)

## Goal

Create a LinkedIn post including:

- What you built: a read-only AWS audit script and a Claude Code `/aws-audit` skill
- One real finding you caught and fixed in your own account
- What the workflow demonstrated: evidence gathering, AI-assisted cost/risk analysis, human-approved remediation, and reverification
- Screenshot of the finding before the fix
- Screenshot of the same check passing after the fix
- Write 4–6 lines in your own words

Suggested tags:

`#DMIByPravinMishra #AWS #AgenticAI #ClaudeCode #DevOps`

### Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

(https://www.linkedin.com/feed/update/urn:li:activity:7498490491521421312/)

---

#### Screenshot of Published LinkedIn Post

![alt text](<LinkedIn Post Screenshot.png>)

---

# Submission Instructions

Complete all tasks in sequence.

Your submission must include:

- All 13 required task screenshots
- Answers to every **Notes You Must Write** question
- `CLAUDE.md`
- `scripts/aws-audit.sh`
- `.claude/skills/aws-audit/SKILL.md`
- `reports/aws-audit-report.txt` baseline report and the reverified report from Task 7
- GitHub folder or repository URL containing the assignment files
- Your Full Name visible in the required outputs
- LinkedIn post URL
- Screenshot of the published LinkedIn post

Submit only a Google Doc link.

Add the GitHub URL inside the Google Doc.

Follow the Assignment Submission Guidelines.

---

# Completion Checklist

- [x] Task 1: AWS resources confirmed and workspace created (Screenshots 1–2)
- [x] Task 2: `CLAUDE.md` created with project context and safety rules (Screenshot 3)
- [x] Task 3: Claude produced a read-only five-check audit plan before any script existed (Screenshot 4)
- [x] Task 4: `aws-audit.sh` built, executable, and passes `bash -n` (Screenshots 5–7)
- [x] Task 5: Baseline audit captured and saved with Full Name visible (Screenshots 8–9)
- [x] Task 6: `/aws-audit` skill loads and runs successfully with no Write permission (Screenshots 10–11)
- [x] Task 7: A real finding was fixed by you and reverified as PASS (Screenshots 12–13)
- [x] Skill never executed a remediation command
- [x] New security group rule is scoped to your own IP, not `0.0.0.0/0`
- [x] All 13 required task screenshots are included
- [x] All "Notes You Must Write" questions are answered in your own words
- [x] No AWS credentials or unblurred account IDs exposed
- [x] LinkedIn post published and URL submitted
- [x] GitHub URL included in the Google Doc
- [x] Google Doc is accessible
- [x] Link tested in incognito mode

---

# Final Submission

Submit only your Google Doc link.

### Question

Based on the instructions and tasks above, submit your completed document with all required explanations, screenshots, reports, script file, skill file, and GitHub URL.

`Add your Google Doc link here`

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