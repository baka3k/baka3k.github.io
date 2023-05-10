---
layout: default
title: 05. Design
parent: 01. Coding checklist
nav_order: 61

---

## 01. Design Class

- [ ] (Recommendation) Follow DI, do not create instance inside classs

- [ ] (Recommendation) Prefer composition over inheritance

- [ ] (Recommendation) Do not use static navigation in xml - instead, use [Kotlin DSL](https://developer.android.com/guide/navigation/navigation-kotlin-dsl)

<br />

## 02. Design Data

- [ ] (Mandatory) Declare API class correctly, use Nullable type for optional property. Default value for network/io response data. For example:

```kotlin
@Serializable
data class NetworkEpisode(
    val name: String = "", // use default value for other property - network/io response 
    val authors: List<String> = listOf(), // use default value for other property - network/io response 
    val alternateVideo: String?, // optional value - use nullable for opptional property 
)
```

<br />

## 03. Design Thread

- [ ] (Mandatory) Do not direct use Thread() of java

- [ ] (Mandatory) Do not run bulk of tasks/threads without Thread Pool