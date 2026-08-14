# Assignment 5 — AI-Assisted Sprint Health Report via Jira MCP

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will connect Claude Code to your Jira board through an MCP server, the same way you connected it to GitHub in Week 2, and build a read-only `/sprint-health` skill. The skill reads your current sprint through Jira's API and reports sprint velocity, stories at risk of missing the sprint, and items missing an estimate — but it must never create, edit, comment on, or transition a single ticket itself. You will prove that boundary holds by making a real change on the board yourself and confirming the skill only ever reports, never acts.

---

# Task 1 — Create a Jira API Token

## Goal

Generate an API token from your Atlassian account that the MCP server will use to authenticate with your Jira site. Do not screenshot the token value itself.

### Evidence

#### Screenshot 1 — Jira API token creation confirmation page showing the token name, with the token value not visible

![alt text](<Week 5_Assignment 05_Screenshot 1.png>)

### Notes You Must Write (Very Important):

Why does the MCP server need your site URL and account email in addition to the token?

The Jira MCP server needs the site URL to identify the correct Jira instance, the account email to identify the Atlassian user, and the API token to authenticate API requests securely. Together, they allow the MCP server to connect to the correct Jira site as the authorized user without using the user's actual account password.

---

# Task 2 — Create .mcp.json at the Project Root

## Goal

Create or update `.mcp.json` at your project root with a Jira MCP server block, following the same shape as the GitHub MCP server you configured in Week 2.

### Evidence

#### Screenshot 2 — `.mcp.json` open in VS Code showing the Jira server configuration

![alt text](<Week 5_Assignment 05_Screenshot 2.png>)

### Notes You Must Write (Very Important):

Compare this jira block to the github block from Week 2 Assignment 5. The GitHub server ran via npx (a Node.js package); this one runs via uvx (a Python package) — what stays exactly the same shape despite that difference, and why doesn't Claude Code care which language a given MCP server is written in?

You can write this:

The Jira and GitHub MCP server blocks keep the same overall structure in `.mcp.json`: each server is defined by a name and includes a `command`, an `args` array, and usually an `env` section for credentials or configuration. The difference is only in how the server is launched: the GitHub MCP server used `npx` for a Node.js package, while the Jira MCP server uses `uvx` for a Python package.

Claude Code does not care which programming language the MCP server is written in because it communicates with the server through the standard **Model Context Protocol (MCP)** interface. As long as the server starts correctly and follows the MCP protocol, Claude Code can discover and use its tools in the same way regardless of whether the server was implemented in Node.js, Python, or another language.


---

# Task 3 — Add Your Credentials to settings.local.json

## Goal

Add your Jira site URL, account email, and API token to `.claude/settings.local.json`, and confirm that file is listed in `.gitignore` so it is never committed.

### Evidence

#### Screenshot 3 — `settings.local.json` open in VS Code showing the `env` section, with the actual token value blurred or covered

![alt text](<Week 5_Assignment 05_Screenshot 3.png>)

### Notes You Must Write (Very Important):

Why must JIRA_API_TOKEN live in settings.local.json and never in .mcp.json?

JIRA_API_TOKEN must be stored in settings.local.json because it is a secret credential that provides authenticated access to Jira. The settings.local.json file is kept local and added to .gitignore, preventing the token from being committed or pushed to GitHub.

.mcp.json, on the other hand, contains the MCP server configuration and may be shared or committed with the project. Putting the API token directly in .mcp.json could accidentally expose the credential through version control. Separating configuration from secrets follows the DevOps security principle of never storing credentials in source code or tracked configuration files.

---

# Task 4 — Verify the Connection with /mcp

## Goal

Restart Claude Code and confirm the Jira MCP server shows as connected.

### Evidence

#### Screenshot 4 — `/mcp` output showing `jira: connected`

![alt text](<Week 5_Assignment 05_Screenshot 4.png>)

---

# Task 5 — Run a Live Query to Prove Real Board Data

## Goal

Ask Claude to list the issues in your current active sprint through the Jira MCP connection, and confirm the result matches what you see on your live board in the browser.

### Evidence

#### Screenshot 5 — Claude's response showing the live sprint issue list retrieved via Jira MCP

![alt text](<Week 5_Assignment 05_Screenshot 6.png>)

### Notes You Must Write (Very Important):

How did you confirm this was real board data and not something Claude guessed?

I confirmed the results were real Jira board data by comparing Claude’s MCP response directly with my live Jira Sprint board in the browser. The issue keys, Story names, statuses, and Story Point estimates matched the issues currently displayed in Sprint 1. Because Claude retrieved the information through the connected Jira MCP server and the returned values matched the live board, I could verify that the response came from Jira’s API rather than being generated or guessed by Claude.

---

# Task 6 — Build the /sprint-health Skill

## Goal

Create a `/sprint-health` skill restricted to read-only Jira tools plus `Read`, with no issue-mutating tools and no `Write`. Run it and confirm it produces a report covering sprint velocity, at-risk stories, and items missing an estimate.

### Evidence

#### Screenshot 6 — `SKILL.md` frontmatter showing `allowed-tools` limited to read-only Jira tools plus `Read`, with `disable-model-invocation: true`

![alt text](<Week 5_Assignment 05_Screenshot 7.png>)

#### Screenshot 7 — `/sprint-health` output showing the full triage report against your real sprint

![alt text](<Week 5_Assignment 05_Screenshot 7-1.png>)

### Notes You Must Write (Very Important):

1. Which Jira MCP tools does this skill's allowed-tools list include, and which mutating tools (create issue, update issue, transition issue, add comment) does it deliberately exclude?

The skill’s `allowed-tools` list includes only these read-only capabilities:

* `Read`
* `mcp__jira__jira_search`
* `mcp__jira__jira_get_issue`
* `mcp__jira__jira_get_agile_boards`
* `mcp__jira__jira_get_sprints_from_board`
* `mcp__jira__jira_get_sprint_issues`

It deliberately excludes mutating Jira tools such as:

* `jira_create_issue`
* `jira_update_issue`
* `jira_transition_issue`
* `jira_add_comment`

That means the skill can inspect Jira data and report sprint health, but it is not permitted to create, edit, comment on, or transition issues.


2. Why does a Scrum Master need this restriction more than almost any other role in this course?

You can write:

A Scrum Master needs this restriction because their primary responsibility is to **facilitate, observe, and improve the Scrum process**, not silently change the team’s work on their behalf. A read-only sprint-health skill preserves **transparency and accountability** by allowing the Scrum Master to identify risks, missing estimates, and progress issues without automatically creating, editing, commenting on, or transitioning Jira tickets. Any actual board changes remain deliberate actions performed by the responsible team members.


---

# Task 7 — Prove the Skill Never Mutates the Board

## Goal

Manually update one ticket on your board in the browser (for example, move a story to "Done" or add a missing estimate), then run `/sprint-health` again and confirm the new report reflects your change — proving the skill only ever reads live state and never wrote to the board itself.

### Evidence

#### Screenshot 8 — Second `/sprint-health` run showing the report now reflects your manual board change

![alt text](<Week 5_Assignment 05_Screenshot 8.png>)

### Notes You Must Write (Very Important):

Map this assignment to Gather → Analyze → Human Act → Verify from Week 3 Assignment 6. Which step did you perform manually in the browser, and why must that step stay human?


Gather: The /sprint-health skill used read-only Jira MCP tools to gather live Sprint 1 data, including issues, statuses, and Story Point estimates.

Analyze: Claude analyzed that data to calculate committed, completed, and remaining Story Points and identify at-risk or unestimated Stories.

Human Act: I manually moved GJOA-6 to Done in the Jira browser. This step must remain human because changing an issue’s status modifies the team’s source of truth and represents an intentional decision about whether work actually meets the team’s completion criteria. The AI should report and recommend, not silently change Scrum records.

Verify: I ran /sprint-health again. It detected GJOA-6 as Done and changed the report from 0 completed / 6 remaining to 1 completed / 5 remaining, confirming that the skill reads live Jira state without modifying it.

This demonstrates the Gather → Analyze → Human Act → Verify pattern: AI provides visibility and analysis, while accountable changes to the Scrum board remain under human control.

---

# Submission Instructions

Complete all tasks in sequence.

Your submission must include:
- All 8 required screenshots
- All the required notes

---

# Completion Checklist

- [x] Task 1: Jira API token created, value never screenshotted (Screenshot 1)
- [x] Task 2: `.mcp.json` has the Jira server block (Screenshot 2)
- [x] Task 3: Credentials stored in `settings.local.json`, token blurred, file gitignored (Screenshot 3)
- [x] Task 4: `/mcp` shows the Jira server connected (Screenshot 4)
- [x] Task 5: Live query returned real sprint data, verified against the browser (Screenshot 5)
- [x] Task 6: `/sprint-health` skill created with correct read-only `allowed-tools`, and produced a full report (Screenshots 6–7)
- [x] Task 7: A manual board change was reflected in a second `/sprint-health` run (Screenshot 8)
- [x] Skill never created, edited, transitioned, or commented on any issue
- [x] Reflection answered (Notes)
- [x] No API token value exposed

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
