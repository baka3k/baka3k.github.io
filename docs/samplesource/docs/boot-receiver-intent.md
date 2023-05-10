---
layout: default
title: BOOT_COMPLETED Intent Notice
parent: 02. Sample Source Code
nav_order: 104
---

Normally, BOOT_COMPLETED will be thrown by System after system finish all preparation tasks ensure System stable.
So, we always get the information late, BOOT_COMPLETED is still deliver late, 

Try to below: 
```
LOCKED_BOOT_COMPLETED
```
-   Official way
-   Limitation: at this time System is not stable and some function is not worked

# Add BOOT_COMPLETED with category tag 

```xml
<intent-filter>
       <category android:name="android.intent.category.DEFAULT" />
       <action android:name="android.intent.action.BOOT_COMPLETED"/>
</intent-filter>

```
-   *Limitation: this is not official way, just a trick, so may not work on some devices*
https://stackoverflow.com/questions/43988566/broadcastreceiver-for-boot-completed-is-too-slow
