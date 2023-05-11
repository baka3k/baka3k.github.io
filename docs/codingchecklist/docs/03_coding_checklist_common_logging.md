---
layout: default
title: 03. Logging & Exception
parent: 01. Coding checklist
nav_order: 52

---

## 01. Logging

- [ ] (Mandatory) Always add log for exception case

```kotlin
    try {
        // todo smth
    }catch (e:Exception){
        Log.e("ClassName","#functionName() err: ${e.message}",e)
        // or  Crashlytics.logException(e)  ... or etc
    }
```

<br />

- [ ] (Mandatory) Do not ignore exception

__BAD__

```kotlin
    try {
        // todo smth
        throw IOException() // assumption that above code throw IOException when error
    }catch (e:Exception) 
    {
       // bad code - ignore exception information
    }
```

__GOOD__
```kotlin
    try {
        // todo smth
        throw IOException() // assumption that above code throw IOException when error
    }catch (e:IOException) 
    {
        Log.e("ClassName","#functionName() err: ${e.message}",e) // or  Crashlytics.logException(e)  ... or etc
        // GOOD add log or send information to server
    }
```

<br />

## 02. Exception

- [ ] (Mandatory) Do not use generic exception

__BAD__

```kotlin
    try {
        // todo smth
        throw IOException() // assumption that above code throw IOException when error
    }catch (e:Exception) // bad code - catch generic exception
    {
        Log.e("ClassName","#functionName() err: ${e.message}",e)
        // or  Crashlytics.logException(e)  ... or etc
    }
```

__GOOD__
```kotlin
    try {
        // todo smth
        throw IOException() // assumption that above code throw IOException when error
    }catch (e:IOException) // just catch Exactly exception from code
    {
        Log.e("ClassName","#functionName() err: ${e.message}",e)
        // or  Crashlytics.logException(e)  ... or etc
    }
```

<br />

- [ ] (Optional) Try throw exception instead of return error for *Critical* issue

__BAD__

```kotlin
fun doSomething():Data{
    if (checkPermission()){
        return Error("have no permission")
    }
    else{
        return getData(/* new data*/)
    }
}
```

__GOOD__

```kotlin
@Throws(SecurityException::class)
fun doSomething():Data{
    if (checkPermission()){
        throw SecurityException("have no permission")
    }
    else{
        return getData(/* new data*/)
    }
}
```