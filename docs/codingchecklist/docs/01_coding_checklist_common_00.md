---
layout: default
title: 01. Process rules
parent: 01. Coding checklist
nav_order: 50
---

## 01. Process rules

|Type|Severity|
|---|--- |
|Mandatory|Must follow, have no exception|
|Recommendation|Should follow, exceptions will be accepted after internal **team make discussion** |
|Optional|Try to follow, make decision based on personal judgment|

# 1. Task description

- [ ] (Mandatory) Jira IDs:<br>
https://xxx.com

- [ ] (Mandatory) IPO relates: <br>
https://xxx.com

- [ ] Detail description: Why/What for this implemenation 

- (Optional) Limitation (Blocked Q&A) <br>
https://xxx.com<br>
https://xxx.com

<br />

# 2. Common rule
- [ ] (Mandatory) Branch name is follow rule (feature/<ID_Task>[/<task_short_description>])
- [ ] (Mandatory) Title of MR is follow rule ([Feature] <ID_Task>: <description>)
- [ ] (Mandatory) Commit message is follow rule.
- [ ] (Mandatory) Source code is rebased from develop branch.
- [ ] (Mandatory) Source code can build without any error.
- [ ] (Mandatory) Don't commit unrelated code to your features.
- [ ] (Mandatory) Run Static Code Analysis at local and fixed all warning errors. (Pending now)

<br />

# 3. Self-review
- [ ] (Mandatory) Format related code.
- [ ] (Mandatory) Remove unused imports.
- [ ] (Mandatory) Variables are declared with ordering: const, final, public, private
- [ ] (Mandatory) Function are declared with ordering: override, public, protected, private
- [ ] (Mandatory) Put related features together for easier tracking(It helps someone reading the class from top to bottom can follow the logic of what's happening).
- [ ] (Mandatory) All classes have copyright?
- [ ] (Mandatory) All source files must be encoded as UTF-8.	
- [ ] (Mandatory) Use static final for constants

<br />

# 4. Self-test 
- [ ] (Mandatory) Current function is working normally
- [ ] (Mandatory) Related function is not affected

<br />

# 5. Solution

- [ ] Optional

# 6. Evidences

- [ ] Optional (image or video here...).

# 7. Release rule
- [ ] (Mandatory) Does new commit rebase with latest source code?
- [ ] (Mandatory) Before create release APK, is app version increased?	