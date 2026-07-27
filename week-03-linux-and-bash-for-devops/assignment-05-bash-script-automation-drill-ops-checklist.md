# Assignment 5 — Bash Script Automation Drill (OPS Checklist)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will practice Bash scripting by building a series of small automation scripts covering environment setup, variables, arrays, loops, file conditionals, if-else logic, and functions. These scripts form the foundation of real-world Linux automation used in DevOps, cloud, and production support environments.

---

# Task 1 — Bash Environment & Workspace Setup

## Goal

Verify that Bash is available on your system and create a clean workspace for this assignment.

### Evidence

#### Screenshot 1 — Output of `echo $SHELL` and `bash --version`

![alt text](<Week 03_Assignment 05_Screenshot 1.png>)

---

#### Screenshot 2 — Output of `pwd` and `ls -lah` showing the scripts directory

![alt text](<Week 03_Assignment 05_Screenshot 2.png>)

---

### Notes

Answer the following in your own words:

**1. What is Bash?**

Bash (Bourne Again Shell) is a command-line interpreter and scripting language used mainly on Linux and Unix systems. It allows users to run commands, automate repetitive tasks, and manage files and system processes through scripts.

---

**2. What is the difference between shell and Bash?**

A shell is a general program that lets users interact with the operating system through commands. Bash is a specific type of shell that provides additional features such as command history, tab completion, scripting capabilities, and improved command editing.

---

**3. Why is it important to confirm the Bash version before writing scripts?**

Checking the Bash version ensures that the features and syntax used in a script are supported by the installed version. This helps avoid compatibility issues and ensures the script runs correctly on the target system.

---

# Task 2 — Your First Bash Script

## Goal

Create your first Bash script, make it executable, and run it from the terminal.

### Evidence

#### Screenshot 1 — Content of `first-script.sh`

![alt text](<Week 03_Assignment 05_Screenshot 4.png>)

---

#### Screenshot 2 — Output of `./first-script.sh`

![alt text](<Week 03_Assignment 05_Screenshot 4-1.png>)

---

#### Screenshot 3 — Output of `ls -l first-script.sh` showing executable permission

![alt text](<Week 03_Assignment 05_Screenshot 5.png>)

---

### Notes

Answer the following in your own words:

**1. What is the purpose of `#!/bin/bash`?**

The #!/bin/bash line, called the shebang, tells the operating system to run the script using the Bash shell. This ensures the script is interpreted correctly and uses Bash features.

---

**2. Why do we use `chmod +x` before running a script?**

The chmod +x command gives a script execute permission, allowing it to be run as a program. Without this permission, the operating system will not allow the script to be executed directly.

---

**3. What is the difference between running a script using `./script.sh` and `bash script.sh`?**

Running ./script.sh executes the script directly and requires the file to have execute permission (chmod +x). Running bash script.sh starts the Bash interpreter and passes the script to it, so execute permission is not required, although the script still needs to be readable.

---

# Task 3 — Variables: User Information Script

## Goal

Use variables to store and display user-related information.

### Evidence

#### Screenshot 1 — Content of `user-info.sh`

![alt text](<Week 03_Assignment 05_Screenshot 6.png>)

---

#### Screenshot 2 — Output of `./user-info.sh`

![alt text](<Week 03_Assignment 05_Screenshot 7.png>)

---

### Notes

Answer the following in your own words:

**1. What is a variable in Bash?**

A variable in Bash is a named storage location used to hold data, such as text, numbers, or command output. It allows you to store values and reuse them throughout a script.

---

**2. Why should we avoid spaces around the `=` sign when creating variables?**

Bash requires variable assignments to have no spaces around the = sign. If spaces are added, Bash treats the variable name and value as separate commands or arguments, which results in an error.

---

**3. How do you access the value stored inside a Bash variable?**

You access the value of a Bash variable by placing a dollar sign ($) before the variable name. For example, if the variable is NAME, you can display its value using echo $NAME.

---

# Task 4 — Arrays & Loops: Tools Checklist Script

## Goal

Use arrays and loops to print a checklist of tools used in Bash scripting.

### Evidence

#### Screenshot 1 — Content of `tools-checklist.sh`

![alt text](<Week 03_Assignment 05_Screenshot 8.png>)

---

#### Screenshot 2 — Output of `./tools-checklist.sh`

![alt text](<Week 03_Assignment 05_Screenshot 9.png>)

---

### Notes

Answer the following in your own words:

**1. What is an array in Bash?**

An array in Bash is a variable that can store multiple values under a single name. Each value is stored at a different index and can be accessed individually.

---

**2. Why are arrays useful in scripts?**

Arrays make it easy to store and manage a list of related items. They allow you to process multiple values without creating a separate variable for each one, making scripts simpler and easier to maintain.

---

**3. What does `"${tools[@]}"` mean?**



---

**4. What is the purpose of the `for` loop in this script?**

The for loop goes through each item in the array one at a time and performs the same action on each element. This avoids repeating code and automates tasks for every item in the list.

---

# Task 5 — Loops: Number Counter Script

## Goal

Use loops to repeat a task multiple times.

### Evidence

#### Screenshot 1 — Content of `counter.sh`

![alt text](<Week 03_Assignment 05_Screenshot 10.png>)

---

#### Screenshot 2 — Output of `./counter.sh`

![alt text](<Week 03_Assignment 05_Screenshot 11.png>)

---

### Notes

Answer the following in your own words:

**1. What is a loop?**

A loop is a programming structure that repeats a block of code multiple times until a condition is met or all items in a list have been processed.

---

**2. Why do we use loops in Bash scripting?**

Loops are used to automate repetitive tasks, reduce duplicate code, and make scripts more efficient by performing the same action multiple times.

---

**3. How many times did the loop run in your script?**

The loop ran five times because we gave it five values:
1 2 3 4 5. It ran once for each number.


---

**4. What would you change if you wanted the loop to run 10 times?**

I would change the loop so that it iterates from 1 to 10. For example, I could use:
for i in {1..10}
This would make the loop execute 10 times.


---

# Task 6 — Files & Conditionals: File Validation Script

## Goal

Use file checks and conditionals to verify whether files and directories exist.

### Evidence

#### Screenshot 1 — Output of `ls -lah ../test-folder`

![alt text](<Week 03_Assignment 05_Screenshot 12.png>)

---

#### Screenshot 2 — Content of `file-check.sh`

![alt text](<Week 03_Assignment 05_Screenshot 13.png>)

---

#### Screenshot 3 — Output of `./file-check.sh`

![alt text](<Week 03_Assignment 05_Screenshot 13-1.png>)

---

### Notes

Answer the following in your own words:

**1. What does `-d` check in Bash?**

The -d test checks whether a specified path exists and is a directory. It returns true if the directory exists.

---

**2. What does `-f` check in Bash?**

The -f test checks whether a specified path exists and is a regular file. It returns true only if the file exists.

---

**3. Why should file and directory paths be stored in variables?**

Storing paths in variables makes scripts easier to read, update, and maintain. If a path changes, you only need to update it in one place instead of throughout the entire script.

---

**4. What happens if the file does not exist?**

If the file does not exist, the -f test returns false, allowing the script to handle the situation, such as displaying an error message or skipping the operation instead of failing unexpectedly.

---

# Task 7 — Conditionals: Pass or Retry Script

## Goal

Use if-else conditionals to make decisions based on a variable value.

### Evidence

#### Screenshot 1 — Content of `score-check.sh` with `score=85`

![alt text](<Week 03_Assignment 05_Screenshot 14.png>)

---

#### Screenshot 2 — Output showing `Result: Pass`

![alt text](<Week 03_Assignment 05_Screenshot 14-1.png>)

---

#### Screenshot 3 — Content of `score-check.sh` with `score=55`

![alt text](<Week 03_Assignment 05_Screenshot 15.png>)

---

#### Screenshot 4 — Output showing `Result: Retry`

![alt text](<Week 03_Assignment 05_Screenshot 15-1.png>)

---

### Notes

Answer the following in your own words:

**1. What is the purpose of if-else in Bash?**

The if-else statement allows a Bash script to make decisions by executing different commands based on whether a condition is true or false.

---

**2. What does `-ge` mean?**

-ge means greater than or equal to. It is used to compare two integer values in a conditional statement.

---

**3. Why should conditions be tested with different values?**

Testing conditions with different values helps ensure the script behaves correctly in different situations. It also helps identify errors and confirms that all possible outcomes work as expected.

---

**4. How can conditionals help in automation scripts?**

Conditionals allow automation scripts to make decisions based on system conditions or user input. This makes scripts more flexible, reliable, and able to handle different scenarios automatically.

---

# Task 8 — Functions: Final Bash Automation Script

## Goal

Create a final Bash script using functions to organize reusable code.

### Evidence

#### Screenshot 1 — Content of `final-automation.sh`

![alt text](<Week 03_Assignment 05_Screenshot 16.png>)

---

#### Screenshot 2 — Output of `./final-automation.sh`

![alt text](<Week 03_Assignment 05_Screenshot 16-1.png>)

---

#### Screenshot 3 — Output of `ls -lah` showing all created scripts

![alt text](<Week 03_Assignment 05_Screenshot 17.png>)

---

### Notes

Answer the following in your own words:

**1. What is a function in Bash?**

A function is a reusable block of code that performs a specific task. Instead of writing the same commands multiple times, you can define them once and call the function whenever needed.

---

**2. Why are functions useful in scripts?**

Functions make scripts easier to read, maintain, and reuse. They reduce duplicate code, simplify troubleshooting, and help organize scripts into smaller, manageable sections.

---

**3. Which functions did you create in this script?**

I created functions to organize different tasks in the script, such as displaying information, checking files or directories, and running the main workflow. Each function handled a specific part of the script.

---

**4. How does this final script combine variables, arrays, loops, conditionals, files, and functions?**

The script stores data in variables, keeps multiple values in arrays, processes them using loops, makes decisions with conditionals (if-else), checks or works with files and directories, and groups related commands into functions. Together, these features make the script organized, reusable, and easier to automate tasks.

---

# LinkedIn Post (Required)

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`https://www.linkedin.com/posts/ossa-agharese-01991a233_devops-linux-bashscripting-share-7484750745934721024-FgDp/?utm_source=share&utm_medium=member_desktop&rcm=ACoAADpVsJUBYBjWiWLSBz1ZojH27wf_yizZYUA`


---

#### Screenshot — Published LinkedIn post

![alt text](<LinkedIn Post_3.png>) 
![alt text](<Week 03_Assignment 04_Screenshot 6-2.png>)

---

# Submission Instructions

- Add all required screenshots in your submission
- Full name must be visible in required screenshots
- All script files must be created and run successfully
- Required notes must be answered clearly for every task
- Do not expose sensitive information (keys, passwords, credentials)

---

# Completion Checklist

- [x] Task 1: Environment setup verified, workspace created (Screenshots 1–2, Notes answered)
- [x] Task 2: First script created, executed, permissions verified (Screenshots 1–3, Notes answered)
- [x] Task 3: Variables script created and run (Screenshots 1–2, Notes answered)
- [x] Task 4: Arrays and loops script created and run (Screenshots 1–2, Notes answered)
- [x] Task 5: Counter loop script created and run (Screenshots 1–2, Notes answered)
- [x] Task 6: File validation script created and run (Screenshots 1–3, Notes answered)
- [x] Task 7: Pass/Retry conditional script tested with both values (Screenshots 1–4, Notes answered)
- [x] Task 8: Final automation script created and run (Screenshots 1–3, Notes answered)
- [x] All scripts run without errors
- [x] Full Name visible in all required screenshots
- [x] LinkedIn post published and URL submitted
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