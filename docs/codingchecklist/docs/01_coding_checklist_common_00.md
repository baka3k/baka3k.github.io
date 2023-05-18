---
layout: default
title: 01. Process rules
parent: 01. Android Coding checklist
nav_order: 50
---

## 01. Process rules

|Type|Severity|
|---|--- |
|Mandatory|Must follow, have no exception|
|Recommendation|Should follow, exceptions will be accepted after internal **team make discussion** |
|Optional|Try to follow, make decision based on personal judgment|

# 1. Task description

- [ ] [Mandatory] Jira IDs:<br>
https://xxx.com

- [ ] [Mandatory] IPO relates: <br>
https://xxx.com

- [ ] Detail description: Why/What for this implemenation 

- (Optional) Limitation (Blocked Q&A) <br>
https://xxx.com<br>
https://xxx.com

<br />

# 2. Common rule
1. - [ ] [Mandatory] Branch name is follow rule (feature/<ID_Task>[/<task_short_description>])
1. - [ ] [Mandatory] Title of MR is follow rule ([Feature] <ID_Task>: <description>)
1. - [ ] [Mandatory] Commit message is follow rule.
1. - [ ] [Mandatory] Source code is rebased from develop branch.
1. - [ ] [Mandatory] Source code can build without any error.
1. - [ ] [Mandatory] Don't commit unrelated code to your features.
1. - [ ] [Mandatory] Run Static Code Analysis at local and fixed all warning errors. (Pending now)

<br />

# 3. Self-review
1. - [ ] [Mandatory] Format related code.
1. - [ ] [Mandatory] Remove unused imports.
1. - [ ] [Mandatory] Variables are declared with ordering: const, final, public, private
1. - [ ] [Mandatory] Function are declared with ordering: override, public, protected, private
1. - [ ] [Mandatory] Put related features together for easier tracking(It helps someone reading the class from top to bottom can follow the logic of what's happening).
1. - [ ] [Mandatory] All classes have copyright?
1. - [ ] [Mandatory] All source files must be encoded as UTF-8.	
1. - [ ] [Mandatory] Use static final for constants

<br />

# 4. Self-test 
1. - [ ] [Mandatory] Current function is working normally
1. - [ ] [Mandatory] Related function is not affected

<br />

# 5. Solution

- [ ] Optional

# 6. Evidences

- [ ] Optional (image or video here...).

# 7. Release rule
1. - [ ] [Mandatory] Does new commit rebase with latest source code?
1. - [ ] [Mandatory] Before create release APK, is app version increased?	