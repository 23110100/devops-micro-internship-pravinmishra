# Week 00 - Internet and Networking

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

# 🧑‍💻 Task 1: Using ChatGPT as Your Learning Assistant

## Scenario

You're new to DevOps and will frequently encounter technical questions. ChatGPT can be your learning companion.

## Your Task

Write a clear ChatGPT prompt to help you understand:

> "What is a protocol in networking? Explain with a simple real-life example."

Take a screenshot of your interaction showing:

* Your detailed prompt (with clear expectations)
* ChatGPT's simplified response with an example

## Screenshot

Save your screenshot in the `screenshots` folder and update the file name below.

![alt text](<Week 00_ Screen shot 1.png>)




---

## What I Learned (2–3 lines)

I learned that a network protocol is a set of rules that allows devices to communicate correctly over a network. I also learned that protocols like HTTP help computers exchange information, just as people follow rules during a conversation.

---

# 🌐 Task 2: Internet and Networking

## Scenario

Your friend is launching an online bookstore named **EpicReads**

He asked you to explain how users globally can access his website hosted in Finland.

## Your Task

Write a short explanation (**100–150 words**) that includes:

* Packet Switching
* IP Address
* TCP/IP
* HTTP/HTTPS

💡 **Tip:** You may use ChatGPT (as demonstrated in Task 1) to refine your explanation.

## Answer

When someone visits the EpicReads website from anywhere in the world, their request travels through the internet to the server in Finland. The information is broken into small pieces called packets, and packet switching helps these packets take the fastest route to the destination. Every device connected to the internet has a unique IP address, which ensures the request reaches the correct server. The communication is managed by TCP/IP, where TCP makes sure all the packets arrive correctly and in the right order, while IP handles the routing. Finally, the browser uses HTTP or HTTPS to load the website. HTTPS is more secure because it encrypts the data, keeping users' personal information safe while they browse or purchase books on EpicReads.

---

# 🏗️ Task 3: Application Architecture & Stack

## Scenario

EpicReads bookstore has two application versions:

### Two-Tier Application

* Frontend
* Database

### Three-Tier Application

* Frontend
* Backend
* Database

## Your Task

* Draw simple diagrams (hand-drawn or tool-based such as draw.io)
* Label each layer clearly
* List at least two common technologies or tools used for each layer
* Submit a screenshot or photo clearly showing your own drawing

## Diagram Screenshot / Photo

![alt text](<Week 00_ Screen shot 2-1.png>)
![alt text](<Week 00_ Screen shot 3-1.png>)
---

## Technologies Used

### Frontend

* HTML, CSS, JavaScript – Used to create and style the user interface of the website.
* React.js – A popular JavaScript library for building interactive and responsive web applications.

### Backend

* Node.js – A JavaScript runtime used to build fast and scalable server-side applications.
* Express.js – A lightweight web framework for Node.js that handles APIs and server logic.

### Database

* MySQL – A relational database used to store structured data such as customer accounts, orders, and book information.
* MongoDB – A NoSQL database used to store flexible, document-based data for modern web applications.

---

# 🌍 Task 4: Domain Name & DNS (Basic Concepts)

## Scenario

Your friend's bookstore **EpicReads** is currently accessible through:

```text
52.172.142.222:3000
```

He purchased the domain:

```text
epicreads.com
```

## Your Task

In **50–100 words**, explain in your own words:

1. What is DNS (Domain Name System)?
2. Which DNS record type should be used to connect the domain to the given IP, and why?

## Answer

DNS (Domain Name System) helps people access websites without having to remember long IP addresses. Instead of typing 52.172.142.222:3000, users can simply type epicreads.com into their browser. To connect the domain to the server's IP address, an A record is used because it points the domain name directly to an IPv4 address. This makes it much easier for people to find and access the EpicReads website while DNS handles the connection behind the scenes.

---

# 💻 Task 5: Visual Studio Code Setup (Hands-on)

## Your Task

Install Visual Studio Code (if not already installed).

Take a screenshot of your VS Code environment showing:

* Terminal open inside VS Code
* Running a basic command:

### Windows

```powershell
dir
```

### Linux / macOS

```bash
pwd
ls
```

* Your selected VS Code theme clearly visible

⚠️ **Important:** The screenshot must show your username or another identifiable detail to confirm it is your environment.

## Screenshot

![alt text](<Week 00_ Screen shot 4.png>)
![alt text](<Week 00_ Screen shot 5.png>)

---

# 🔗 Task 6: Publish Your Assignment as a LinkedIn Post

## Objective

Publishing on LinkedIn helps you:

* Build your professional online presence
* Reinforce your learning
* Document your DevOps journey publicly

## Your Task

Summarize your answers from Tasks 1–5 into a LinkedIn post.

Clearly structure your post into the following sections:

* ChatGPT
* Internet & Networking
* App Architecture
* DNS
* VS Code Setup

Add the following credit note at the end of your post:

> **P.S. This post is part of the DevOps Micro Internship (DMI) with Agentic AI — Cohort 3 — by Pravin Mishra. My graded progress is public: https://dmi.pravinmishra.com/s/YOUR-GITHUB-USERNAME.html · Start your DevOps journey: https://dmi.pravinmishra.com/?utm_source=student&utm_medium=ps-linkedin&utm_campaign=cohort3**

---

## LinkedIn Post URL

https://www.linkedin.com/posts/ossa-agharese-01991a233_dmi-devops-micro-internship-with-agentic-share-7487933022722396160-ajul/?utm_source=share&utm_medium=member_desktop&rcm=ACoAADpVsJUBYBjWiWLSBz1ZojH27wf_yizZYUA

---

## LinkedIn Post Backup Copy

🚀 Strengthening My Networking & Application Architecture Fundamentals | DevOps Learning Journey

Even as a Senior DevOps Engineer, I believe continuous learning is essential. This week, I revisited core networking and application architecture concepts through practical scenarios and hands-on exercises, reinforcing the foundations that support modern cloud-native applications.

🤖 ChatGPT

I explored how ChatGPT can be used as a technical learning assistant to simplify complex topics, validate my understanding, and refine technical explanations. Used effectively, it complements—not replaces—hands-on experience and critical thinking.

🌐 Internet & Networking

I refreshed my knowledge of how web applications are accessed globally. From packet switching and IP addressing to the TCP/IP protocol suite and HTTP/HTTPS, these technologies work together to ensure reliable, efficient, and secure communication between users and servers.

🏗️ Application Architecture

I compared two-tier and three-tier application architectures and reviewed the responsibilities of each layer:

Frontend: React.js, HTML/CSS/JavaScript
Backend: Node.js, Express.js
Database: MySQL, MongoDB

Understanding these layers is fundamental when designing scalable, maintainable, and cloud-ready applications.

🌍 DNS

I revisited how the Domain Name System (DNS) translates user-friendly domain names into IP addresses. I also reviewed how an A Record maps a domain directly to an IPv4 address, making websites accessible without users needing to remember numerical IPs.

💻 VS Code Setup

I configured my development environment in Visual Studio Code, installed essential extensions, and organized my workspace to improve productivity when developing infrastructure, automation, and cloud-native solutions.

Although these are foundational concepts, they remain critical for designing resilient cloud infrastructure, troubleshooting production environments, and building reliable DevOps pipelines.

Continuous learning is one of the best investments any technology professional can make.

#DevOps #AWS #Azure #CloudComputing #Networking #DNS #TCPIP #HTTP #DevSecOps #PlatformEngineering #SRE #InfrastructureAsCode #ContinuousLearning #VSCode #GitHub #AgenticAI #DevOpsMicroInternship

**P.S. This post is part of the DevOps Micro Internship (DMI) with Agentic AI — Cohort 3 — by Pravin Mishra. My graded progress is public: https://dmi.pravinmishra.com/s/YOUR-GITHUB-USERNAME.html · Start your DevOps journey: https://dmi.pravinmishra.com/?utm_source=student&utm_medium=ps-linkedin&utm_campaign=cohort3

---

# Reflection – Week 0

### What did you find easy?

Since I already have experience as a DevOps Engineer, I found the GitHub setup, repository management, and basic networking concepts straightforward. The tasks also helped reinforce foundational DevOps concepts that I use in my daily work.

---

### What was difficult?

The most challenging part was ensuring that every submission met the internship's formatting and grading requirements, including organizing screenshots, maintaining the correct folder structure, updating Markdown files, and documenting each task accurately. It highlighted the importance of attention to detail in technical documentation

---

### What will you improve next week?

Next week, I plan to complete the assignments earlier, review the submission checklist before pushing my changes to GitHub, and continue strengthening my knowledge of DevOps fundamentals while applying industry best practices. I also intend to make my documentation more concise and ensure every deliverable is complete before submission

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.


## 📌 Resources

- 🌐 **DMI Official Website:** https://pravinmishra.com/dmi  
- 🎓 **DevOps for Beginners (Udemy):** https://www.udemy.com/course/devops-for-beginners-docker-k8s-cloud-cicd-4-projects/  
- 🎓 **Ultimate Agentic AI DevOps with Clude Code** https://www.udemy.com/course/ultimate-agentic-ai-devops-with-claude-code/?referralCode=448389767BC96284087B
- 🎓 **DevOps with Claude Code: Terraform, EKS, ArgoCD & Helm** https://www.udemy.com/course/devops-with-claude-code-terraform-eks-argocd-helm/?referralCode=1C5B734505D65A010FA3
- ▶️ **YouTube Playlist (DMI Cohort 3):** https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 **Pravin Mishra (LinkedIn):** https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 **CloudAdvisory (LinkedIn):** https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track*