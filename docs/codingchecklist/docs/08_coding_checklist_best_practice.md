---
layout: default
title: 08. Performance
parent: 01. Coding checklist
nav_order: 64

---

## 01. Compose

- [ ] (Mandatory) Defer reads as long as possible

__BAD:__
```kotlin
fun CompassView(compassStateProvider: () -> Float) {
   
    Canvas(
        modifier = Modifier
            .width(LocalConfiguration.current.screenWidthDp.dp)
            .height(LocalConfiguration.current.screenWidthDp.dp)
            .rotate(-compassStateProvider()) // bad do not use this function - Compose recomposes many times
    )
}
```

__GOOD:__
```kotlin

fun CompassView(compassStateProvider: () -> Float) {
    Canvas(
        modifier = Modifier
            .width(LocalConfiguration.current.screenWidthDp.dp)
            .height(LocalConfiguration.current.screenWidthDp.dp)
             .graphicsLayer {
                // prevent recompose by defer state update 
                rotationZ = -compassStateProvider()
            }
    )
}
```
<br />
- [ ] (Mandatory) Use lazy layout keys

__BAD:__

```kotlin
@Composable
fun NotesList(notes: List<Note>) {
    LazyColumn {
        items(
            items = notes
        ) { note ->
            NoteRow(note)
        }
    }
}
```
__GOOD:__

```kotlin
@Composable
fun NotesList(notes: List<Note>) {
    LazyColumn {
        items(
            items = notes,
            key = { note ->
                // Return a stable, unique key for the note
                note.id
            }
        ) { note ->
            NoteRow(note)
        }
    }
}
```
<br />
- [ ] (Mandatory) Use derivedStateOf to limit recompositions

__BAD:__
```kotlin
@Composable
fun BadView() {
    val data = mutableListOf<Int>()
    for (i in 1..1000) {
        data.add(i)
    }
    val listState = rememberLazyListState()
    val showButton = listState.firstVisibleItemIndex > 0
    Log.d("test", "BadView()") // recompose many times while scroll list items
    Column {
        Log.d("test", "Column()") // recompose many times while scroll list items
        if (showButton) {
            Button(onClick = { /*TODO*/ }) {
                Text(text = "Hello meo meo")
            }
        }
        LazyColumn(state = listState) {
            items(items = data) { item ->
                Text(text = "$item")
            }
        }
    }
}
```

__GOOD:__
```kotlin
@Composable
fun GoodView() {
    val data = mutableListOf<Int>()
    for (i in 1..1000) {
        data.add(i)
    }
    val listState = rememberLazyListState()
    val showButton by remember {
        derivedStateOf { // use derivedStateOf to prevent recompose
            listState.firstVisibleItemIndex > 0
        }
    }
    Column {
        if (showButton) {
            Button(onClick = { /*TODO*/ }) {
                Text(text = "Hello meo meo")
            }
        }
        LazyColumn(state = listState) {
            items(items = data) { item ->
                Text(text = "$item")
            }
        }
    }
}
```
<br />
- [ ] (Mandatory) Avoid backwards writes

__BAD:__

```kotlin 
@Composable
fun BadComposable() {
    var count by remember { mutableStateOf(0) }

    // Causes recomposition on click
    Button(onClick = { count++ }, Modifier.wrapContentSize()) {
        Text("Recompose")
    }

    Text("$count")
    count++ // BAD: Backwards write, writing to state after it has been read - Re-compose again & again
}

```