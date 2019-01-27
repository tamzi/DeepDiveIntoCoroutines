An ATM system has many atms.


1. Recording transactions per ATM:

```
private static int balance = 0;

private static void deposit() {
    balance = balance + 1;
}
private static void withdraw() {
    balance = balance - 1;
}
```


How to do it in concurrency.
```
private fun atmSystem() {
  for (i in 0 until TRANSACTIONS_PER_MACHINE)
  {
    deposit() // put a dollar in
    withdraw() // take it back out
    System.out.println(balance) // makes the bug disappear!
  }
}
```


Implement coroutines