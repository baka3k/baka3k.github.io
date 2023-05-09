---
layout: default
title: BOOT_COMPLETED Intent Notice
parent: 02. Sample Source Code
nav_order: 104
---

Normally, BOOT_COMPLETED will be thrown by System after system finish all preparation tasks ensure System stable.
So, we always get the information late, BOOT_COMPLETED is still deliver late, 

Try to below: 
# LOCKED_BOOT_COMPLETED
-   Official
-   Limitation: at this time System is not stable and some function is not worked
# BOOT_COMPLETED with category tag 
-   *Limitation: without official document, may not work on some devices*
![alt text](https://i.ibb.co/QPV7FfX/Screenshot-2023-03-06-at-09-06-20.png)
https://stackoverflow.com/questions/43988566/broadcastreceiver-for-boot-completed-is-too-slow
