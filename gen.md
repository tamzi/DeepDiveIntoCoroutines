All coroutine in Kotlin is done by implementing:

***
```
suspend fun validateEntries(str: String): Boolean =
    coroutineScope {
        val deferred1 = async { firstValidation(str) }
        val deferred2 = async { secondValidation(str) }
        deferred1.await() && deferred2.await()
    }
```