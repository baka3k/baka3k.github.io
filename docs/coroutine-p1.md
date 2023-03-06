---
layout: default
title: Coroutine - part 1
nav_order: 9
---

# Coroutine check list (in progress)

## Inspiration

* [Coroutine](https://kotlinlang.org/docs/reference/coroutines-overview.html)
## Không sử dụng GlobalScope 
+ Ko sử dụng GlobalScope - mà ko quản lý - với **bất cứ** lý do gì

__BAD:__

```kotlin
GlobalScope.launch {
  // to do something
}
```

__GOOD:__

```kotlin
val coroutineScope = CoroutineScope(coroutineContext) // tạo mới coroutine scope follow lifecycle
coroutineScope.launch {
  // to do something  
}
// cancel follow lifecycle
coroutineScope.cancel() 
```
## withContext() vs async-await


__BAD:__

```kotlin
val defer1 = async {// do some thing}
val result1 = defer1.await()

val defer2 = async {// do some thing}
val result2 = defer2.await()
```
__GOOD:__

```kotlin
 val result1 = withContext(coroutineContext){// do some thing}
 val result2 = withContext(coroutineContext){// do some thing}
```
**async-async-await-await** <= *chỉ sử dụng async trong trường hợp cần chạy song song như thế này*
## Không khởi tạo thêm Scope
+ Ko khởi tạo thêm scope ko cần thiết 

__BAD:__

```kotlin
internal suspend fun loadUrl(path: String): Int = suspendCoroutine { continuation ->
        CoroutineScope(Dispatchers.Main).launch { // khởi tạo CoroutineScope(Dispatchers.Main) <= ko cần thiết
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
private suspend fun loadUrl(path: String): Int = withContext(Dispatchers.Main) { // chuyển sang Dispatchers.Main
        suspendCancellableCoroutine<Int> {continuation-> //suspendCancellableCoroutine thường được sử dụng cho các trường hợp 'nắn' callback thành suspend function
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
                        continuation.resumeWith(Result.success(0)) // resume coroutine - 'nắn' callback thành suspend function

                    }
                }
                it.loadUrl(path)
            }
        }
//        0
    }
```

## Prefer dùng suspendCancellableCoroutine hơn là suspendCoroutine


__BAD:__

```kotlin
suspend fun doSomeThing():State{
      return suspendCoroutine { suspendCoroutine ->
           // do some thing
      }
}
```
__GOOD:__

```kotlin
suspend fun doSomeThing():State{
      return suspendCancellableCoroutine { suspendCancellableCoroutine ->
           // do some thing
      }
}
```

## Channel 
+ Channel dùng cho việc gửi nhận giữa các Coroutine
+ Channel dùng làm blocking queue
=> Ngoài hai lý do ý, ko nên dùng channel. Channel nên được dùng đúng với chức năng mà nó được tạo ra

__BAD:__

```kotlin
suspend fun doSomeThing():State{
      val channel = Channel<State>()
      val job1 = CoroutineScope(Dispatchers.Main + supervisor).launch {
                  // to do some thing
                  channel.send(state1) // (1)
              }
      val job2 = CoroutineScope(Dispatchers.Default + supervisor).launch {
                  // to do some thing
               channel.send(state2) // (2)
      }
      val state = channel.receive() // suspend current coroutine and wait data send from (1) hoặc (2) - item nào đến trước thì cancel cái còn lại
      job1.cancel()  
      job2.cancel() 
      channel.close()
      // do something with state
      return state
}
```
Đoạn lệnh trên dùng channel với một lý do DUY NHẤT là suspend - chờ các state gửi đến, nhận state sớm nhất , nên có thể đổi bằng 1 cách khác như dưới
__GOOD:__

```kotlin
suspend fun doSomeThing():State{
      return suspendCancellableCoroutine { suspendCancellable ->
            CoroutineScope(Dispatchers.Main + supervisor).launch { // Scope 1
                  // to do some thing
                  if (suspendCancellable.isActive) {
                      suspendCancellable.resumeWith(Result.success(state1))
                      suspendCancellable.cancel()
                  }
              }
              CoroutineScope(Dispatchers.Default + supervisor).launch { // Scope 2
                  // to do some thing
                  if (suspendCancellable.isActive) {
                      suspendCancellable.resumeWith(Result.success(state2))
                      suspendCancellable.cancel()
                  }
              }
      }
}
```
## Hãy cẩn thận khi dùng Chanel
+ Chú ý việc cancel channel: channel.send() và channel.receive() đều suspend current coroutine(coroutine gọi ra nó), khi gọi channel.cancel() - sender coroutine & receive coroutine đều bị cancel() - nếu sender và receiver là root coroutine - các job sẽ bị huỷ theo
+ channel là hot state, nên việc sử dụng channel không kiểm soát có thể dẫn đến vấn đề liên quan đến leak memory

## Credits

- [baka3k](baka3k@gmail.com)

