---
layout: default
title: 02. Commons
parent: 01. Android Coding checklist
nav_order: 51

---

## 01. Coding conventions

- [ ] (Mandatory) follow coding convention 
https://developer.android.com/kotlin/style-guide


## 02. Naming conventions

- [ ] (Mandatory) Do not use naming beginning or ending with special characters(underline, dollar mark...etc)

- [ ] (Mandatory) Don't use local language(Japanese/Vietnamese...) naming for class name, function name, property, parameter variable (unless it sounds like English, e.g. Okinawa, Hanoi)

- [ ] (Mandatory) Class name must be defined by UpperCamelCase format. Except in the case of abbreviations, for example:DAO, UID, DTO, 
    Avoid unnatural simplifications for readability

        * Incorrect : AbsClass, Condi, int a, val b
        * Correct : AbstractClass,  Condition, int count, val user

- [ ] (Mandatory) Method name, parameter name, member variable name and local variable name are described by lowerCamelCase

- [ ] (Mandatory) Abstract class names start with Abstract or Base

- [ ] (Mandatory) Exception class name ends with Exception

- [ ] (Mandatory) Test class name ends with Test

- [ ] (Mandatory) Package is specified in lowercase. After each dot, there is only 1 single English word. Package name is in singular

- [ ] (Recommendation) Method name, Variable name must be meaningful. Do not use Method/Variable such as temp, test, a, b..etc

