# Assignment 6 — AI-Assisted Terraform Drift and Policy Review

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Student Details

**Full Name:** Osamudiamen Agharese  
**GitHub Repository/Folder URL:** https://github.com/23110100/devops-micro-internship-pravinmishra.git

---

## Purpose

Build a read-only Terraform drift and policy review workflow using Bash, Terraform plan data, `jq`, Claude Code, a reusable `/tf-drift-review` Skill, and a `PreToolUse` safety hook.

The workflow must follow this pattern:

```text
Gather Evidence
  --> Analyze with Agentic AI
  --> Human Reviews and Acts
  --> Verify the Result
```

The `/tf-drift-review` Skill and `tf-drift-check.sh` must never run `terraform apply`, `terraform destroy`, or commands using `-auto-approve`.

---

# Task 1 — Confirm the Clean Baseline and Create the Workspace

## Goal

Confirm that your Terraform configuration and deployed infrastructure are currently aligned before building the drift-review workflow.

## Evidence

### Screenshot 1 — Clean Terraform Plan

Add a screenshot of `terraform plan` showing no pending changes.

![alt text](<Week 08 Assignment 6_Screenshort 1.png>)

---

### Screenshot 2 — Assignment Workspace

Add a screenshot of the folder structure showing `AI Assignment/`, `reports/`, and the Terraform project.

![alt text](<Week 08 Assignment 6_Screenshort 2.png>)

## Questions

### 1. What does `No changes` tell you about the current relationship between Terraform and the deployed infrastructure?

`No changes` means Terraform compared the current configuration and state with the deployed AWS infrastructure and found no differences that require action. This confirms that the infrastructure is currently aligned with the Terraform configuration and provides a clean baseline before testing for drift.


### 2. Why is a clean baseline important before introducing a test change?

A clean baseline is important because it confirms there is no existing drift before introducing a test change. This makes it easier to prove that any difference detected afterward was caused by the intentional test change rather than a pre-existing infrastructure issue.


---

# Task 2 — Create Project Context and Safety Rules in `CLAUDE.md`

## Goal

Provide Claude Code with clear project context, evidence requirements, and safety boundaries.

## Evidence

### Screenshot 3 — Project Context and Safety Rules

Add a screenshot of `CLAUDE.md` open in VS Code showing the Project Overview, Review Workflow, Safety Rules, and Output Rules.

![alt text](<Week 08 Assignment 6_Screenshort 3.png>)

## Questions

### 1. Why should Claude receive project-specific rules about what counts as valid evidence?

Claude needs project-specific rules so its conclusions are based on approved Terraform plan data, structured JSON, and collected project evidence rather than assumptions. This makes the drift review consistent, traceable, and evidence-based.

### 2. Why must the human remain responsible for running `terraform apply`?

The human must remain responsible for terraform apply because it can make real infrastructure changes. Claude can analyze evidence and recommend remediation, but the engineer must review the plan, assess the impact and risks, and explicitly decide whether the change should be applied.

### 3. Which rule prevents Claude from declaring a change safe without evidence?

The rule requiring Terraform plan output and structured evidence to be reviewed before any infrastructure-changing action prevents Claude from declaring a change safe without evidence. It ensures recommendations are supported by collected evidence and remain subject to human review.

---

# Task 3 — Build the Terraform Drift and Policy Check Script

## Goal

Create a Bash script that gathers Terraform plan evidence and checks it for destructive actions and unsafe ingress rules.

## Evidence

### Screenshot 4 — Script Variables and Checks Array

Add a screenshot of the top section of `tf-drift-check.sh` showing the variables and `checks` array.

![alt text](<Week 08 Assignment 6_Screenshort 4.png>)

---

### Screenshot 5 — Destructive-Action and Open-Ingress Checks

Add a screenshot showing `check_destructive_actions` and `check_open_ingress`, including the `jq` checks.

![alt text](<Week 08 Assignment 6_Screenshort 5.png>)

---

### Screenshot 6 — Script Validation and Permissions

Add a screenshot showing successful `bash -n` and `ls -l` output.

![alt text](<Week 08 Assignment 6_Screenshort 6.png>)

## Questions

### 1. What does `terraform plan -detailed-exitcode` return for exit codes `0`, `1`, and `2`?

Exit code 0 means the plan completed successfully and no changes were detected. Exit code 1 means Terraform encountered an error. Exit code 2 means the plan completed successfully but detected changes between the configuration and infrastructure.

### 2. Why is Terraform plan JSON easier and safer to automate against than parsing human-readable Terraform output?

Terraform plan JSON provides structured and predictable fields that tools such as jq can query directly. Human-readable plan output is designed for people and its formatting can change, making text parsing more fragile. JSON allows the script to reliably inspect resources, actions, and configuration values without depending on terminal formatting.

### 3. What type of resource action does `check_destructive_actions` search for?

check_destructive_actions searches the Terraform plan JSON for resource changes whose actions array contains a delete action. This identifies resources Terraform plans to destroy or replace.

### 4. Why does finding a `delete` action also help detect replacements?

Terraform replacements normally involve deleting the existing resource and creating a new one. Therefore, a replacement action includes delete in the resource's actions array, such as ["delete","create"] or ["create","delete"]. Searching for delete helps detect both direct destruction and replacement operations.

### 5. Why must this script never run `terraform apply`?

The script is designed only to gather and analyze evidence. Running terraform apply would allow the automated workflow to modify real infrastructure and bypass the required human review step. Keeping apply out of the script preserves the workflow: Gather Evidence → Analyze with Agentic AI → Human Reviews and Acts → Verify the Result.

---

# Task 4 — Run the Script Against the Clean Baseline

## Goal

Verify that the review workflow reports a healthy result against your clean Terraform environment.

## Evidence

### Screenshot 7 — Healthy Baseline Report

Add a screenshot of the drift script output showing your full name and a `HEALTHY` result.

![alt text](<Week 08 Assignment 6_Screenshort 7.png>)

---

### Screenshot 8 — Baseline Script Exit Code

Add a screenshot showing the captured script exit code `0`.

![alt text](<Week 08 Assignment 6_Screenshort 8.png>)

## Questions

### 1. What is the Overall Status of your baseline?

The current overall status is REVIEW REQUIRED. The drift-review script completed successfully, but Terraform detected two in-place launch-template changes caused by the selected AMI changing. No destructive actions or unsafe ingress rules were detected.

### 2. Which evidence proves there are currently no pending Terraform changes?

The original clean baseline terraform plan showed “No changes. Your infrastructure matches the configuration.” However, the latest run no longer proves there are no pending changes. It returned Terraform exit code 2 and Plan: 0 to add, 2 to change, 0 to destroy, indicating two pending AMI-related updates.

### 3. Was `reports/tfplan.json` created? Explain why or why not.

No. The script created reports/terraform-plan.json, not reports/tfplan.json, because the PLAN_JSON variable was configured with the filename terraform-plan.json. The JSON was generated from the saved Terraform plan using terraform show -json. If the assignment specifically requires reports/tfplan.json, the script should be updated to use that exact filename.

---

# Task 5 — Create and Run the `/tf-drift-review` Claude Code Skill

## Goal

Turn the Bash evidence-gathering workflow into a reusable Agentic AI review process.

## Evidence

### Screenshot 9 — `/tf-drift-review` Skill Configuration

Add a screenshot of `SKILL.md` showing the frontmatter, allowed tools, and safety rules.

![alt text](<Week 08 Assignment 6_Screenshort 9.png>)

---

### Screenshot 10 — Clean Agentic AI Review

Add a screenshot of `/tf-drift-review` showing the clean `HEALTHY` result.

![alt text](<Week 08 Assignment 6_Screenshort 10.png>)

## Questions

### 1. Why does this Skill have `Bash`, `Read`, and `Grep`, but not `Write`?

The Skill uses Bash, Read, and Grep because its purpose is to gather and analyze existing Terraform evidence. It does not need Write because the review is intentionally read-only and should not automatically modify Terraform configuration or infrastructure.

### 2. Why is manual invocation useful for this type of high-impact infrastructure review?

Manual invocation keeps the engineer in control of when the review runs. For high-impact infrastructure work, this provides a clear human decision point and prevents automated actions from occurring without the engineer's knowledge and review.

### 3. Which part of the workflow is deterministic Bash automation?

The Bash script performs the deterministic evidence-gathering work. It runs Terraform validation and planning, captures the detailed exit code, converts the saved plan to JSON, and uses jq to check for destructive actions and unsafe ingress rules.

### 4. Which part requires Claude's reasoning?

Claude's reasoning is used to interpret the collected evidence, understand the context of detected changes, distinguish expected behavior from potential risks, explain findings, and recommend appropriate next steps for human review.

### 5. Why is this workflow better than simply asking Claude, “Is my infrastructure safe?”

This workflow gives Claude structured evidence from the actual Terraform environment instead of asking it to make a broad judgment from assumptions. Terraform and jq provide deterministic evidence, Claude analyzes that evidence, and the human engineer retains control over any remediation. This makes the process more traceable, repeatable, and evidence-based.

---

# Task 6 — Introduce a Controlled Difference and Detect It

## Goal

Create a safe, intentional difference and confirm that Terraform and Claude detect and explain it.

## Evidence

### Screenshot 11 — Controlled Difference

Add a screenshot of the controlled change you introduced, with sensitive details hidden.

![alt text](<Week 08 Assignment 6_Screenshort 11.png>)

---

### Screenshot 12 — Detected Difference and Risk Assessment

Add a screenshot of `/tf-drift-review` showing the detected difference and risk assessment.

![alt text](<Week 08 Assignment 6_Screenshort 12.png>)

---

### Screenshot 13 — Detected Drift Report

Add a screenshot of `drift-detected-report.txt` showing your full name and the `WARN` or `FAIL` result.

![alt text](<Week 08 Assignment 6_Screenshort 13.png>)

## Questions

### 1. What change did you introduce?
I intentionally changed the Web Auto Scaling Group Name tag in Terraform from bookreview-web to bookreview-web-drift-test to create a safe, controlled difference for testing the review workflow.

### 2. Was it true infrastructure drift or a Terraform configuration change?

It was a Terraform configuration change, not true infrastructure drift. I changed the desired configuration locally but did not apply it to AWS, so the deployed infrastructure remained unchanged.

### 3. What Terraform plan evidence proves that a change is pending?

The Terraform plan showed Plan: 0 to add, 1 to change, 0 to destroy and returned detailed exit code 2. It specifically showed the Web Auto Scaling Group Name tag changing from bookreview-web to bookreview-web-drift-test.

### 4. Was the action an update, deletion, replacement, or security-rule change?

It was an in-place update to the Web Auto Scaling Group tag. There were no deletions, replacements, or security-rule changes.

### 5. What did Claude recommend?

Claude recommended treating the detected difference as requiring human review before any action was taken. The change was low risk and non-destructive, but the workflow correctly stopped without running terraform apply.

### 6. Why should you review the recommendation before taking action?

AI recommendations should be checked against the actual Terraform plan, project requirements, and operational impact. Human review ensures that the proposed action is intentional and safe before any infrastructure-changing command is executed.

---

# Task 7 — Add a `PreToolUse` Hook to Block Unsafe Apply Attempts

## Goal

Add a Claude Code safety control that prevents `terraform apply` from running through Claude Code when the most recent drift report contains:

```text
Overall Status: FAIL
```

## Evidence

### Screenshot 14 — `PreToolUse` Safety Hook

Add a screenshot of `.claude/settings.json` showing the `PreToolUse` safety hook.

![alt text](<Week 08 Assignment 6_Screenshort 14.png>)

---

### Screenshot 15 — Blocked Apply Attempt

Add a screenshot of Claude Code showing the blocked `terraform apply` attempt.

![alt text](<Week 08 Assignment 6_Screenshort 15.png>)

## Questions

### 1. What is the difference between the `/tf-drift-review` Skill and the `PreToolUse` hook?

The /tf-drift-review Skill gathers and analyzes Terraform evidence and explains detected changes and risks. The PreToolUse hook is an enforcement control that checks commands before execution and can block unsafe actions such as terraform apply.

### 2. Which component performs analysis?

The /tf-drift-review Skill performs the analysis by using Claude to interpret Terraform plan evidence, policy findings, and detected differences.

### 3. Which component enforces the safety gate?

The PreToolUse hook enforces the safety gate. It deterministically blocks terraform apply when the latest drift report contains Overall Status: FAIL.

### 4. Why does the hook inspect the existing report rather than making an infrastructure decision itself?

The hook is designed to enforce an existing review result rather than perform AI reasoning. The Skill analyzes the evidence first, records the status, and the hook uses that status as a deterministic input for deciding whether a high-impact command should be blocked.

### 5. Why is a deterministic guard useful for high-impact commands?

A deterministic guard applies the same safety rule every time and does not depend on AI interpretation at the moment a command is executed. For high-impact commands such as terraform apply, this provides an additional predictable control that can prevent infrastructure changes when the required safety conditions have not been met.

---

# Task 8 — Resolve the Difference and Verify the Final State

## Goal

Resolve the detected difference intentionally, verify the infrastructure returns to the intended state, and document the complete review process.

## Evidence

### Screenshot 16 — Human-Reviewed Resolution

Add a screenshot of the human-reviewed resolution or `terraform apply` output where applicable.

![alt text](<Week 08 Assignment 6_Screenshort 16.png>)

---

### Screenshot 17 — Final Healthy Review

Add a screenshot of the final `/tf-drift-review` showing `HEALTHY`.

![alt text](<Week 08 Assignment 6_Screenshort 17.png>)

---

### Screenshot 18 — Saved Reports

Add a screenshot of `ls -lah reports` showing both:

- `drift-detected-report.txt`
- `resolved-report.txt`

![alt text](<Week 08 Assignment 6_Screenshort 18.png>)

---

### Screenshot 19 — Drift Review Summary

Add a screenshot of `drift-review-summary.md` showing all required sections and your full name.

![alt text](<Week 08 Assignment 6_Screenshort 19.png>)

## Terraform Drift Review Summary

### 1. Change Introduced

Explain the controlled change you introduced.

State whether it was:

- True infrastructure drift, or
- A Terraform configuration change

I intentionally changed the Web Auto Scaling Group Name tag from bookreview-web to bookreview-web-drift-test. This was a Terraform configuration change, not true infrastructure drift, because the change was made locally and was never applied to AWS.

### 2. Evidence Collected

Describe the Terraform plan evidence and affected resource.

Terraform detected the difference in module.web.aws_autoscaling_group.web. The plan showed:

Plan: 0 to add, 1 to change, 0 to destroy

and returned detailed exit code 2, confirming that a change was pending.

### 3. Risk Assessment

Explain the risk identified by the Bash check and Claude Code.

The Bash checks found no destructive/replacement actions and no unsafe open-ingress changes. Claude reviewed the evidence and identified the ASG tag modification as a low-risk, in-place update that still required human review before any infrastructure change.

### 4. Human-Approved Action

Explain the action you reviewed and executed manually.

I reviewed the proposed change and decided not to apply the test configuration to AWS. I manually restored the ASG Name tag from bookreview-web-drift-test back to the intended bookreview-web value.

### 5. Verification

Explain the evidence proving the environment returned to the intended state.

After restoring the configuration, I ran the drift-review workflow again. Terraform returned exit code 0, with no pending changes, no destructive actions, and no unsafe ingress findings. The final result was HEALTHY and was saved in resolved-report.txt.

### 6. Safety Decision

Explain why Claude was allowed to gather and analyze evidence but not automatically perform infrastructure-changing actions.

Claude was allowed to gather evidence, inspect Terraform plans, and explain risks because these are read-only activities. Infrastructure-changing actions remained under human control. The PreToolUse hook provided an additional deterministic safety gate to block terraform apply when the review status was FAIL.

### 7. Agentic Loop Mapping

Explain how your workflow followed:

```text
Gather --> Analyze --> Human Act --> Verify
```

The workflow followed the required loop:

Gather → Analyze → Human Act → Verify

Gather: Bash and Terraform collected plan and policy evidence.
Analyze: /tf-drift-review used Claude to interpret the evidence and assess risk.
Human Act: I reviewed the recommendation and manually restored the intended Terraform configuration.
Verify: I reran the workflow and confirmed Terraform plan exit code: 0 and a final HEALTHY result.

## Questions

### 1. What action did you execute to resolve the difference?
I manually restored the Web Auto Scaling Group Name tag in the Terraform configuration from bookreview-web-drift-test back to the intended bookreview-web value. Because the test change had never been applied to AWS, no terraform apply was required.
Write your answer here.

### 2. Did you review `terraform plan` before taking action?

Yes. I reviewed the Terraform plan first and confirmed that the proposed action was an in-place update with 0 to add, 1 to change, 0 to destroy before deciding how to resolve it.

### 3. What evidence proves the environment is now aligned?

The final Terraform review returned exit code 0 and reported No changes, confirming that the deployed infrastructure matches the Terraform configuration. The final drift review also returned HEALTHY, and the result was saved in resolved-report.txt.

### 4. Why is a second drift review required after the fix?

A second review verifies that the resolution actually restored the intended state and did not introduce additional differences, destructive actions, or policy violations.

### 5. What could go wrong if an AI agent automatically applied every detected Terraform change?

It could apply unintended, destructive, insecure, or costly changes without sufficient human review. A detected difference does not automatically mean that applying it is the correct remediation.

### 6. In one sentence, explain the difference between asking an AI chatbot “Is my infrastructure okay?” and using this evidence-based Agentic AI workflow.

A general chatbot question relies mainly on available context, while this Agentic AI workflow gathers real Terraform evidence, analyzes it against defined policies, keeps infrastructure changes under human control, and verifies the final state.

---

# LinkedIn Post — Mandatory

## Goal

Publish a LinkedIn post in your own words describing:

- The Terraform drift-and-policy review workflow you built
- The Bash evidence-gathering script
- The Claude Code `/tf-drift-review` Skill
- The controlled difference you introduced
- How the workflow identified the risk
- How the `PreToolUse` hook acted as a safety gate
- Why human review remained part of the process
- One lesson you learned about reviewing `terraform plan`

Include a screenshot of the detected change and a screenshot of the final `HEALTHY` review in your post.

Suggested tags:

```text
#DMIByPravinMishra #Terraform #AgenticAI #ClaudeCode #DevOps
```

## LinkedIn Evidence

### LinkedIn Post URL

https://www.linkedin.com/feed/update/urn:li:share:7508571871202742272/

### Published LinkedIn Post Screenshot — Mandatory

![alt text](<Week 08 Assignment 6_Screenshort 20.png>)

---

# Required Assignment Files

Confirm that the following files are included in your GitHub repository:

- `CLAUDE.md`
- `AI Assignment/tf-drift-check.sh`
- `.claude/skills/tf-drift-review/SKILL.md`
- `.claude/settings.json` containing the safety hook
- `reports/drift-detected-report.txt`
- `reports/resolved-report.txt`
- `drift-review-summary.md`

---

# Submission Instructions

- Complete Tasks 1–8 in sequence.
- Include Screenshots 1–19 exactly as specified.
- Answer every question under Tasks 1–8 in your own words.
- Complete all seven sections of the Terraform Drift Review Summary.
- Include the GitHub repository/folder URL containing the assignment files.
- Include your full name in the required reports and screenshots.
- Include the LinkedIn post URL and a screenshot of the published LinkedIn post.
- Do not expose access keys, passwords, tokens, account IDs, private keys, Terraform secrets, or other sensitive information.
- Review all screenshots carefully and hide or redact sensitive details where necessary.

---

# Completion Checklist

- [x] Created the required assignment workspace
- [x] Created or updated `CLAUDE.md`
- [x] Added project context and safety rules
- [x] Created `tf-drift-check.sh`
- [x] Added my full name to the report
- [x] Validated the Bash script
- [x] Made the script executable
- [x] Used `terraform plan -detailed-exitcode`
- [x] Used Terraform plan JSON
- [x] Used `jq` to inspect destructive actions
- [x] Used `jq` to inspect unsafe ingress
- [x] Confirmed the baseline returns `HEALTHY`
- [x] Created `/tf-drift-review`
- [x] Restricted the Skill to appropriate tools
- [x] Confirmed the Skill remains read-only
- [x] Confirmed the Skill never runs `terraform apply`
- [x] Confirmed the Skill never runs `terraform destroy`
- [x] Introduced a controlled detectable difference
- [x] Correctly identified whether it was true drift or a configuration change
- [x] Saved `drift-detected-report.txt`
- [x] Added the `PreToolUse` safety hook
- [x] Verified the hook blocks `terraform apply` when the report is `FAIL`
- [x] Reviewed the Terraform evidence before resolving the change
- [x] Performed any infrastructure-changing action manually
- [x] Ran the drift review again after resolution
- [x] Confirmed the final status is `HEALTHY`
- [x] Saved `resolved-report.txt`
- [x] Completed `drift-review-summary.md`
- [x] Mapped the workflow to `Gather --> Analyze --> Human Act --> Verify`
- [x] Included all 19 numbered screenshots
- [x] Answered all required questions
- [x] Published the required LinkedIn post
- [x] Added the LinkedIn post URL and screenshot
- [x] Included the GitHub repository/folder URL
- [x] Confirmed that no sensitive information is exposed

---

*This submission is part of the DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*
