---
layout: default
title: 02. Commons
parent: 01. Android Coding checklist
nav_order: 51

---

## 01. Coding conventions

- [ ] [Mandatory] follow coding convention 
https://developer.android.com/kotlin/style-guide


## 02. Naming conventions

1. - [ ] [Mandatory] Do not use naming beginning or ending with special characters(underline, dollar mark...etc)
1. - [ ] [Mandatory] Don't use local language(Japanese/Vietnamese...) naming for class name, function name, property, parameter variable (unless it sounds like English, e.g. Okinawa, Hanoi)
1. - [ ] [Mandatory] Class name must be:
     - Defined by UpperCamelCase format, except in the case of abbreviations, for example:DAO, UID, DTO 
     - Avoid unnatural simplifications for readability
        - Incorrect : AbsClass, Condi, int a, val b
        - Correct : AbstractClass,  Condition, int count, val user

1. - [ ] [Mandatory] Method name, parameter name, member variable name and local variable name are described by lowerCamelCase

1. - [ ] [Mandatory] Abstract class names start with Abstract or Base

1. - [ ] [Mandatory] Exception class name ends with Exception

1. - [ ] [Mandatory] Test class name ends with Test

1. - [ ] [Mandatory] Package is specified in lowercase. After each dot, there is only 1 single English word. Package name is in singular

1. - [ ] [Recommendation] Method name, Variable name must be meaningful. Do not use Method/Variable such as temp, test, a, b..etc

