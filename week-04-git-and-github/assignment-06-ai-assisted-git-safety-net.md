# Assignment 6 — Building an AI-Assisted Git Safety Net (PR Ready Check)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In Week 2 you built Claude Code hooks that block a dangerous action *before* it happens (`PreToolUse`), and a restricted skill that could look but not touch (`allowed-tools` without `Write`). In this assignment you will discover that Git has the exact same idea, decades older: a **pre-commit hook** that blocks a commit before it's created.

You will build both halves of a real "PR Ready" workflow:

1. A **Git hook that follows fixed rules** — scans staged changes for hardcoded secrets and oversized files and refuses the commit. No AI involved, no guessing, just a rule that gives the same answer every time.
2. A **restricted Claude Code skill** (`/pr-ready`) that reads your staged diff and drafts a Pull Request title, description, and a short list of things worth a second look — the kind of judgment a fixed rule can't make (mixed changes, missing context, unclear intent). The skill never commits, pushes, or opens the PR. You do that yourself, using its draft as a starting point.

This mirrors the Agentic Loop from Week 3's Linux triage assignment: **Gather → Analyze → Human Act → Verify**. The hook and the skill both gather and analyze; only you act.

---

# Task 0 — Confirm Your Fork and Create a Feature Branch

## Goal

Confirm you are working in your own fork, then create a dedicated branch for this assignment.

### Evidence

#### Screenshot 1 — Output of git remote -v and git branch showing the new branch

![alt text](<screenshots/Week 4 Assignment 6_Screenshot 1.png>)

---

### Notes

**1. Why create a dedicated branch instead of doing this work on main?**

Creating a dedicated branch instead of working directly on `main` helps protect the stable codebase, allows safe development and testing, and supports collaboration through Pull Requests. It enables code review, easier troubleshooting, and reduces the risk of introducing bugs into production.

**In short:** Feature branches provide a safer and more organized way to develop, review, and deploy changes.


---

# Task 1 — Stage a Change With Realistic Risk

## Goal

On your own fork of this repository (the one you've been submitting your DMI work in since onboarding), create a new branch and stage a change that a real reviewer should catch: a hardcoded-looking secret and a leftover debug statement.

### Evidence

#### Screenshot 1 — Output of  `git status` showing the staged file on feature/ai-pr-ready

![alt text](<screenshots/Week 4 Assignment 6_Screenshot 1.1png.png>)

---

### Notes

**1. Why does this assignment use an obviously fake key instead of a real one?**

The assignment uses a fake key because using a real credential in a practice project could create a security risk. It helps demonstrate how to handle secrets safely without exposing sensitive information. In real-world DevOps environments, actual keys should never be committed to a repository; they should be stored securely using tools like environment variables, secrets managers, or CI/CD secret storage.


---

# Task 2 — Write a Real Git Pre-Commit Hook

## Goal

Create a tracked, shareable pre-commit hook that blocks a commit containing secret-like patterns or files over 1MB.

### Evidence

#### Screenshot 2 — `hooks/pre-commit` open in VS Code showing the full script

![alt text](<screenshots/Week 4 Assignment 6_Screenshot 2.png>)

---

#### Screenshot 3 — Output of `git config core.hooksPath` confirming it points to `hooks`

![alt text](<screenshots/Week 4 Assignment 6_Screenshot 3.png>)

---

### Notes

**1. Why is `hooks/pre-commit` tracked in the repo instead of living only in `.git/hooks/`?**

`hooks/pre-commit` is kept in the repository so that everyone working on the project can use the same rules and checks. The `.git/hooks/` folder only exists on an individual developer’s computer and does not get shared when the project is cloned.

By keeping the hook in the repo, team members can easily access it, review it, and maintain the same standards before code is committed. This helps prevent mistakes and keeps the development process consistent for everyone.


---

**2. Compare this to `PreToolUse` from Week 2 Assignment 6. What does each one intercept, and what do they have in common?**

`hooks/pre-commit` and `PreToolUse` work in a similar way because they both act as a checkpoint before something happens.

A `pre-commit` hook checks changes before they are saved into Git history. It can help catch problems like bad formatting, failed tests, or accidentally committing sensitive information.

`PreToolUse` from Week 2 Assignment 6 checks an AI agent’s request before it runs a tool or performs an action. It helps make sure the action is safe and follows the expected rules.

The main thing they have in common is that they both **stop and check an action before allowing it to happen**. They help improve security, prevent mistakes, and make workflows more reliable.


---

# Task 3 — Prove the Hook Blocks the Risky Commit

## Goal

Attempt to commit the staged file from Task 1 and show the hook rejecting it.

### Evidence

#### Screenshot 4 — Terminal showing `git commit` rejected with the hook's "BLOCKED" message naming the exact file

![alt text](<screenshots/Week 4 Assignment 6_Screenshot 4.png>)

---

### Notes

**1. Which line in `hooks/pre-commit` matched your fake key, and why did it match?**

The fake key was detected by the line that uses grep to search for patterns that look like sensitive information. It matched because the fake key started with AKIA and followed the same format as an AWS access key. Even though it wasn't a real key, the hook recognized the pattern and blocked the commit to help prevent secrets from being added to the repository.

---

**2. Could this hook have caught a poorly-named variable that stores a secret without the `AKIA` prefix? What does that tell you about the limits of a fixed rule like this?**

No, not necessarily. This hook only looks for specific patterns, such as keys that start with **`AKIA`** or private key headers. If a secret was stored in a poorly named variable that didn't match those patterns, the hook could easily miss it and allow the commit.

This shows that fixed rules have limitations. They are useful for catching known patterns, but they can't detect every type of secret. That's why it's important to combine them with other security tools and good development practices to better protect sensitive information.


---

# Task 4 — Build the `/pr-ready` Skill

## Goal

Create a manually invoked Claude Code skill that reads your staged changes and produces a PR-readiness report and a draft PR description — without writing, committing, or pushing anything itself.

### Evidence

#### Screenshot 5 — `SKILL.md` frontmatter showing `allowed-tools: Bash, Read, Grep` (no `Write`) and `disable-model-invocation: true`

![alt text](<Week 4 Assignment 6_Screenshot 7.png>)

---

#### Screenshot 6 — `/pr-ready` output while the risky file is still staged, showing it flagged the secret and/or debug statement

![alt text](<screenshots/Week 4 Assignment 6_Screenshot 5.png>)

---

### Notes

**1. Why does `/pr-ready` have `Bash` and `Read` but not `Write`?**

`/pr-ready` has `Bash` and `Read` because it only needs to check and review the project before a pull request. It uses `Bash` to run commands like checking Git status or running tests, and it uses `Read` to look at files and understand the changes.

It does not have `Write` because the purpose of the skill is not to edit or create files. Keeping `Write` disabled prevents accidental changes and makes sure the tool only reviews the code and gives feedback without modifying the project.


---

**2. The pre-commit hook and `/pr-ready` both looked at the same staged diff. Did they flag the same things? What did one catch that the other didn't?**

No, the pre-commit hook and `/pr-ready` did not flag exactly the same things. They both checked the staged changes, but they had different purposes.

The pre-commit hook used a simple rule-based check to look for specific problems, such as patterns that looked like exposed secrets. It only caught issues that matched the rules it was programmed to detect.

`/pr-ready` performed a broader review of the changes. It looked at the overall quality of the pull request, including code changes, documentation, possible issues, and whether the changes were ready to be submitted.

The pre-commit hook was better at quickly blocking obvious problems, while `/pr-ready` provided a more complete review. For example, the hook could catch a fake AWS key pattern, but `/pr-ready` could notice things like missing documentation, unclear changes, or areas that needed improvement before opening a pull request.


---

# Task 5 — Fix the Issues and Re-Verify

## Goal

Remove the secret and debug statement, then prove both gates now pass clean.

### Evidence

#### Screenshot 7 — `git commit` succeeding after the fix (no BLOCKED message)

![alt text](<screenshots/Week 4 Assignment 6_Screenshot 6.png>)

---

#### Screenshot 8 — Second `/pr-ready` run showing a clean risk report and a drafted PR title + description

![alt text](<screenshots/Week 4 Assignment 6_Screenshot 7.png>)

---

### Notes

**1. What exactly did you change to satisfy the pre-commit hook?**

I removed the fake AWS key from the `scripts/notify.sh` file and replaced it with a simple demo message. The script no longer contains any sensitive-looking credentials, so the pre-commit hook no longer blocks the commit. I changed it to just display a notification message without using any secrets.


---

# Task 6 — Push and Open a Pull Request Using the AI Draft

## Goal

Push your branch and open a real Pull Request, using `/pr-ready`'s drafted title and description as your starting point — read it critically and edit before you use it.

**Important:** Open this Pull Request with base repository set to **your own fork** — not the shared upstream `pravinmishraaws/devops-micro-internship-pravinmishra` repository. This assignment's hook and skill files are your own practice work, not a change meant for the shared class repo.

### Evidence

#### Screenshot 9 — Your Pull Request showing the base repository is your own fork, plus the title and description, with the `/pr-ready` draft visible for comparison (paste it in the PR conversation or your notes below)

![alt text](<screenshots/Week 4 Assignment 6_Screenshot 8-1.png>)

---

#### PR Link

https://github.com/pravinmishraaws/devops-micro-internship-interviews/pull/386

---

### Notes

**1. What, if anything, did you edit in the AI's drafted PR description before using it? Why?**

I reviewed the AI-generated PR description and made small edits to ensure it accurately reflected the work I completed. I updated the summary, confirmed that the checklist matched the tasks I actually performed, and removed or changed any statements that were not applicable. This made the PR accurate, clear, and truthful.

---

**2. If you had blindly copy-pasted the AI's draft without reading it, what could go wrong?**

The PR could contain incorrect or misleading information, such as claiming I completed validation steps I did not run or describing changes I did not make. This could confuse reviewers, reduce trust in my work, and result in requests for corrections or rejection of the pull request.

---

**3. Why does this PR need to target your own fork instead of the shared upstream repository?**

The PR needs to target my own fork because I do not have write access to the shared upstream repository. My fork is where I develop and submit my changes for review. This keeps the upstream repository protected, allows maintainers to review contributions safely, and follows the standard open-source contribution workflow.

---

# Task 7 — Map the Workflow to the Agentic Loop

## Goal

Explain this assignment's workflow using the same Gather → Analyze → Human Act → Verify structure from Week 3.

### Notes

**1. Which step(s) represent Gather?**

The Gather step was when I reviewed the assignment instructions, explored the repository, and used AI to help draft the interview question and PR description. I also collected the information and references needed to complete the assignment.

---

**2. Which step(s) represent Analyze?**

The Analyze step was reviewing the AI's suggestions, checking that the content was accurate, making edits where needed, and ensuring the answers followed the assignment requirements. I also evaluated the results of the pre-commit hook to confirm it worked correctly.

---

**3. Which step is Human Act, and why must a human — not Claude — run `git commit`, `git push`, and open the PR?**

The Human Act step was when I committed my changes, pushed my branch to GitHub, and created the pull request. These actions affect my GitHub repository, so I needed to review everything first and take responsibility for what was submitted. AI can provide guidance, but only I should decide when and what to commit and submit.

---

**4. Which step is Verify?**

The Verify step was checking that the pre-commit hook blocked the fake secret, confirming my branch was pushed successfully, reviewing the pull request before submitting it, and making sure all the required files and changes were included.

---

**5. In one or two sentences: why do you need *both* the fixed-rule pre-commit hook and the AI skill? Isn't one enough?**

The pre-commit hook catches known problems automatically using fixed rules, such as detecting fake AWS keys before a commit is made. The AI skill provides broader feedback by reviewing content, explanations, and potential improvements that fixed rules cannot detect, so using both gives better overall quality and security.

---

# Task 8 — LinkedIn Post

## Goal

Publish a LinkedIn post summarizing what you built and what you learned about combining fixed-rule safety checks with AI-assisted review.

### Evidence

#### LinkedIn Post URL

https://www.linkedin.com/posts/ossa-agharese-01991a233_dmi-devops-micro-internship-with-agentic-share-7488038451494268928-C-7j/?utm_source=share&utm_medium=member_desktop&rcm=ACoAADpVsJUBYBjWiWLSBz1ZojH27wf_yizZYUA

---

## Key Learnings

Add 3-5 bullet points on what you learned this week.

-Learned how to combine AI-assisted reviews with traditional DevOps workflows to improve productivity while maintaining engineering quality.

-Gained hands-on experience implementing and testing Git pre-commit hooks to prevent secrets from being committed to a repository.

-Reinforced the importance of reviewing and validating AI-generated content instead of accepting it without verification.

---

# Submission Instructions

- Ensure `hooks/pre-commit` and `.claude/skills/pr-ready/SKILL.md` are committed to your GitHub repository
- Add all required screenshots to your submission
- All written answers must be in your own words
- Do not use a real secret or credential anywhere in your submission — the fake key in Task 1 is intentional and must stay clearly fake
- Open your Pull Request against your own fork, not the shared upstream repository
- Push your final changes to your forked repository
- Include your PR link and LinkedIn post URL

---

## GitHub Repository URL

Paste your forked repository URL here:

(https://github.com/23110100/devops-micro-internship-interviews.git)

---

# Completion Checklist

- [ ] Branch `feature/ai-pr-ready` created with a staged file containing a fake secret and a debug statement
- [ ] `hooks/pre-commit` created and tracked in the repo (not only in `.git/hooks/`)
- [ ] `core.hooksPath` configured to point at `hooks/`
- [ ] Pre-commit hook shown blocking the risky commit
- [ ] `.claude/skills/pr-ready/SKILL.md` created with correct `allowed-tools` (no `Write`) and `disable-model-invocation: true`
- [ ] `/pr-ready` run against the risky diff and shown flagging issues
- [ ] Risky file fixed; `git commit` succeeds cleanly
- [ ] `/pr-ready` re-run showing a clean report and drafted PR title/description
- [ ] Pull Request opened using the AI draft as a starting point, with your own fork as the base repository (not upstream), PR link included
- [ ] Agentic Loop mapping (Task 7) completed in your own words
- [ ] LinkedIn post published and URL submitted
- [ ] All required screenshots added
- [ ] GitHub repository URL provided

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
