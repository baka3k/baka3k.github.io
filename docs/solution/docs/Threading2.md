# Threading and Lies (Part 2)

## "Adding more threads will make it faster"

Many people believe this — especially when we’re starting out. It sounds logical: more threads, more speed. But in reality, it’s not always true.

Let’s explore this with a classic example: finding prime numbers. A prime number is only divisible by 1 and itself. Here’s a simple Kotlin function to check if a number is prime:

```kotlin
private fun checkPrime(number: Int): Boolean {
    if (number == 0 || number == 1 || number == 2 || number == 3 || number == 5 || number == 7) {
        return true
    } else if (number == 4 || number == 6 || number == 8 || number == 9) {
        return false
    }
    val max = sqrt(number.toDouble()).toInt()
    for (i in 2..max) {
        if (number % i == 0) {
            return false
        }
    }
    return true
}
```

### Finding primes from 0 to 100,000

```kotlin
private fun findPrime(number: Int) {
    for (i in 0..number) {
        if (checkPrime(i)) {
            // to do smth
        }
    }
}
private fun caculatePrimeTime(number: Int) {
    val start = System.currentTimeMillis()
    findPrime(number)
    val spentTime = System.currentTimeMillis() - start
    val sNumber = DecimalFormat().format(number)
    println("$sNumber Calculation time $spentTime")
}
fun testPrime() {
    coroutineScope.launch {
        val array = intArrayOf(
            50,
            20000,
            200000,
            2000000,
            20000000,
        )
        array.forEach {
            caculatePrimeTime(it)
        }
    }
}
```

### SingleThread Output (Mac Mini M1):

- **50** → Calculation time: `0 ms`
- **20,000** → Calculation time: `2 ms`
- **200,000** → Calculation time: `10 ms`
- **2,000,000** → Calculation time: `157 ms`
- **20,000,000** → Calculation time: `3886 ms`

Now, we want to speed up - let's use ***more Threads** - right? 
And... the lie begins
## Multi-threading Approach

Two common ways to parallelize this:

1. **Divide the number ranges into smaller parts** — e.g., thread 1 processes 1-5000, thread 2 processes 5001-10,000, and so on.
2. **Thread pool approach** — Each number is checked by an available thread in a pool.

Prime numbers are non-linear, so the first approach can lead to uneven workloads among threads. The second approach tends to balance work better.

Let’s try Kotlin Coroutines with `Dispatchers.Default` (ignoring CPU core count and thread idling checks for now):

```kotlin
suspend fun findPrimesWithCoroutines(number: Int) {
    coroutineScope {
        for (i in 0..number) {
            launch(Dispatchers.Default) {
                if (checkPrime(i)) {
                    // Do something with the prime number
                }
            }
        }
    }
}
```

### MultiThread Output:

- **50** → Calculation time: `2 ms`
- **20,000** → Calculation time: `55 ms`
- **200,000** → Calculation time: `144 ms`
- **2,000,000** → Calculation time: `560 ms`
- **20,000,000** → Calculation time: `6364 ms`

### Side-by-side Performance Comparison

| Number Range       | SingleThread Time (ms) | MultiThread Time (ms) |
|--------------------|-------------------------|------------------------|
| **50**             | `0`                     | `2`                    |
| **20,000**         | `2`                     | `55`                   |
| **200,000**        | `10`                    | `144`                  |
| **2,000,000**      | `157`                   | `560`                  |
| **20,000,000**     | `3886`                  | `6364`                 |

WHAT????, Single thread is faster than Multithread- what the heck is going on???"

### Understanding the bottleneck

For smaller workloads, thread overhead (scheduling, context switching) can outweigh the performance gains. But let’s push the workload higher:

### Large-scale comparison

**SingleThread:**

- **200,000,000** → Calculation time: `102,918 ms`
- **2,000,000,000** → Calculation time: `5,122,890 ms`

**MultiThread:**

- **200,000,000** → Calculation time: `65,939 ms`
- **2,000,000,000** → Calculation time: `1,051,704 ms`

At this scale, multi-threading wins decisively. The thread management cost becomes insignificant compared to the computation.

## Performance Comparison Table

| Number Range       | SingleThread Time (ms) | MultiThread Time (ms) |
|--------------------|-------------------------|------------------------|
| **50**             | `0`                     | `2`                    |
| **20,000**         | `2`                     | `55`                   |
| **200,000**        | `10`                    | `144`                  |
| **2,000,000**      | `157`                   | `560`                  |
| **20,000,000**     | `3886`                  | `6364`                 |
| **200,000,000**    | `102,918`               | `65,939`               |
| **2,000,000,000**  | `5,122,890`             | `1,051,704`            |

## So, does adding threads make it faster?

**It depends.** For lightweight tasks, thread overhead may slow things down. For heavier tasks, multi-threading can deliver massive speedups.

Sometimes, “more threads = faster” is a lie — but other times, it’s the key to performance gains.

*Feel free to leave comments and discuss this topic openly!*
