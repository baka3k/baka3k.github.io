---
layout: default
title: 07. MultiThreading
parent: 01. Android Coding checklist
nav_order: 63

---

## 01. CoroutineScope

1. - [ ] [Mandatory] Do not use `GlobalScope`

    __BAD:__
    ```kotlin
    GlobalScope.launch {
    // to do something
    }
    ```
    __GOOD:__
    ```kotlin
    val coroutineScope = CoroutineScope(coroutineContext + SupervisorJob()) // or using coroutine scope follow lifecycle
    coroutineScope.launch {
    // to do something  
    }
    // cancel follow lifecycle
    coroutineScope.cancel() 
    ```
1. - [ ] [Mandatory] Do not create new coroutineScope
1. - [ ] [Mandatory] prefer use `suspendCancellableCoroutine` instead of `suspendCoroutine`

    __BAD:__

    ```kotlin
    private suspend fun loadUrl(path: String): Int = suspendCoroutine { continuation ->
            CoroutineScope(Dispatchers.Main).launch { // BAD create new CoroutineScope(Dispatchers.Main)
                webView?.let {
                    it.webViewClient = object : WebViewClient() {
                        override fun shouldOverrideUrlLoading(
                            view: WebView,
                            request: WebResourceRequest
                        ): Boolean {
                            it.loadUrl(request.url.toString())
                            return true
                        }
                        override fun onPageFinished(view: WebView?, url: String?) {
                            isLoadURL = true
                            continuation.resume(0)
                        }
                    }
                    it.loadUrl(path)
                }
            }
            //
        }
    ```
    __GOOD:__
    ```kotlin
    private suspend fun loadUrl(path: String): Int = withContext(Dispatchers.Main) { 
            suspendCancellableCoroutine<Int> {continuation-> //suspendCoroutine => suspendCancellableCoroutine
                webView?.let {
                    it.webViewClient = object : WebViewClient() {
                        override fun shouldOverrideUrlLoading(
                                view: WebView,
                                request: WebResourceRequest
                        ): Boolean {
                            it.loadUrl(request.url.toString())
                            return true
                        }
                        override fun onPageFinished(view: WebView?, url: String?) {
                            isLoadURL = true
                            continuation.resumeWith(Result.success(0))

                        }
                    }
                    it.loadUrl(path)
                }
            }
    }
    ```


## 02. Race Condition

1. - [ ] [Mandatory] Do not use comparison condition equals  `'=='` as final break in multithreading environment

__BAD:__

```kotlin
var i = 100
val coroutineScope = CoroutineScope(Dispatchers.IO + SupervisorJob())
fun test() {
    i = 100
    for (i in 1..5) {
        coroutineScope.launch {
            raceCondition() // run in multitheading
        }
    }
}

fun raceCondition() {
    while (true) {
        if (i == 50) { // error in this line - 'Race Condition' Error is still to happen 
            break
        }
        println("i $i")
        i -= 1
    }
}
```
__GOOD:__

```kotlin
var i = 100
val coroutineScope = CoroutineScope(Dispatchers.IO + SupervisorJob())
fun test() {
    i = 100
    for (i in 1..5) {
        coroutineScope.launch {
            raceCondition() // run in multitreading
        }
    }
}

fun raceCondition() {
    while (true) {
        if (i <= 50) { // used '<=' instead of '=='
            break
        }
        println("i $i")
        i -= 1
    }
}
```


## 03. Channel