---
layout: default
title: 05. Design
parent: 01. Android Coding checklist
nav_order: 61

---

## 01. Design Class

1. - [ ] [Recommendation] Follow DI, do not create instance inside class

__BAD:__
```kotlin
class BarcodeDecoder(context: Context) {
    private val display = Display(context)
    // to do smth
}
```

__GOOD:__
```kotlin
class BarcodeDecoder(private val display: Display) {
    // to do smth
}
```
1. - [ ] [Recommendation] Prefer composition over inheritance

1. - [ ] [Recommendation] Do not use static navigation in xml - instead, use [Kotlin DSL](https://developer.android.com/guide/navigation/navigation-kotlin-dsl)

<br />

## 02. Design Data

1. - [ ] [Mandatory] Declare API class correctly, use Nullable type for optional property. Default value for network/io response data. For example:

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

1. - [ ] [Mandatory] Do not direct use `Java#Thread()` 

1. - [ ] [Mandatory] Do not run bulk of tasks/threads without Thread Pool