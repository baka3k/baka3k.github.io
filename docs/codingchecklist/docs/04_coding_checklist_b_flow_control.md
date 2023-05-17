---
layout: default
title: 04. Flow control
parent: 01. Android Coding checklist
nav_order: 53

---




1. [Mandatory] Do not invoke `remove/add` inside `foreach` . Try to use `Iterater#remove` / `Iterator#add` or `Iterator#filter`

1. [Mandatory] if we override the equals() method, we also have to override hashCode().

1. [Mandatory] Element of Set MUST be to implemented override `equals` & `hashCode`

1. [Mandatory] Key of Map  MUST be to implemented override `equals` & `hashCode`

1. [Mandatory] final of switch-case must have `default`

1. [Mandatory] Constant variable must be `const val`

1. [Recommendation] Method prepare to delete. set annotation `@Deprecated`

1. [Recommendation] Do not use `+` to appen String, should use `StringBuilder#append()`

1. [Recommendation] Scope of access modifier set minimum as much as possible

1. [Recommendation] Discuss with the team to make decision if create Utility Class (ScreenUtlil, StringUtils... etc)

1. [Recommendation] Do not use Deprecated API (`@Deprecated`)

1. [Recommendation] Prefer composition over inheritance

1. [Recommendation] When reluctant to write code of type `if()...else if()...else...`, the maximum is `3` level

1. [Recommendation] For ease of understanding, try not to use the `!` . operator. Limit the use of negative clauses, use positive clauses

1. [Recommendation] To improve readability, do not write complex conditional judgment directly with if statement, instead, make complex conditional judgment result into boolean variable, and then write in if statement
__BAD:__
```kotlin
if ((file.open(fileName, "w") != null) && (...) || (...)) {
        ...
}
```
__GOOD:__
```kotlin
boolean existed = (file.open(fileName, "w") != null) && (...) || (...);
if (existed) {
        ...
}  
```

1. [Mandatory] Do not init value in contructor of customview, should wait until inflate layout finish
__BAD:__
```kotlin
class BadInitCustomView : View {
    constructor(context: Context?, attributeSet: AttributeSet?, defStyleAttr: Int) : super(context,attributeSet,defStyleAttr) {
        init(context!!, attrs = attributeSet, defStyleAttr = defStyleAttr)
    }

    private fun init(context: Context, attrs: AttributeSet?, defStyleAttr: Int) {
        // to do smt
    }
}
```
__GOOD:__
```kotlin
class GoodInitCustomView(
    context: Context?,
    private val attributeSet: AttributeSet?,
    private val defStyleAttr: Int
) : View(context, attributeSet, defStyleAttr) {

    private fun init(context: Context, attrs: AttributeSet?, defStyleAttr: Int?) {
        // to do smt
    }
    override fun onFinishInflate() {
        super.onFinishInflate()
        init(context, attrs = attributeSet, defStyleAttr = defStyleAttr) // wait until inflate layout finish
    }
}
```

1. [Recommendation] Prefer to use collectAsStateWithLifecycle over collectAsState for UI state
__BAD:__
```kotlin
val uiNowPlayingUiState by viewModel.nowPlayingUiState.collectAsState()
```
__GOOD:__
```kotlin
val uiNowPlayingUiState by viewModel.nowPlayingUiState.collectAsStateWithLifecycle()
```