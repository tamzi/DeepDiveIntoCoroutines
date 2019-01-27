With callbacks

```
fun <A> inTransaction(f: (Connection) -> CompletableFuture<A>)
                            : CompletableFuture<A> {  
    return this.sendQuery("BEGIN").flatMap { _ ->    
        val p = CompletableFuture<A>()      
        f(this).onComplete { ty1 ->      
            sendQuery(
                if (ty1.isFailure) "ROLLBACK" else "COMMIT") 
            .onComplete { ty2 ->        
                if (ty2.isFailure && ty1.isSuccess)           
                    p.failed((ty2 as Failure).exception)        
                else          
                    p.complete(ty1)      
            }    
        }    
        p  
   }
}

```


Withoout callbacks: Coroutines implemented:

```
suspend fun <A> inTransaction(
                   f: suspend (Connection) -> A): A {    
    try {      
        this.sendQuery("BEGIN")      
        val result = f(this)      
        this.sendQuery("COMMIT")      
        return result    
    } catch (e: Throwable) {      
        this.sendQuery("ROLLBACK")      
        throw e    
    }   
}
```