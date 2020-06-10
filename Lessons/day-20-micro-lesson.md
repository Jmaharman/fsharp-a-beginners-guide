# Day 20: micro-lesson
I have generally used `Option` and `List` in the form you are used to in C#, but in F# they are more often described as below:

```fsharp
type Customer = {
    Email : string option
    Orders : string list
}
// Equivalent to the above
type CustomerAlt = {
    Email : Option<string>
    Orders : List<string>
}
```
[Try the code](https://try.fsharp.org/#?code=C4TwDgpgBAwgrgZ2AewLYQE5QLxQN4CwAUFKVAKKoCGAlgDZQBcUSGNAdgOZTJjA3J2xMlADyGACaYETFsDZcodGkmIBfYsQD0WigEc4NAG5U6EdsCgorAC2hUARsiMRioSLEQp0GAIJ1LXEISMkpaBmZRPgF2AB5WDk4APmEycSkMGWYAGRVgePlElKINImIzSwBjLwBGWXgkNEwcfFTSMPoWgGUmqAAiACtkG3YAAQQAaxAAdypJAFoEZDo4fkEEADpKtD62sUlpFoBtfvTmmr6AXXVNIgqoaqQAJnqvJr8AluCRDoZcHvQ-SGI3GU1mCyWKzW7E221QuxCpDOmWOpwOWAu1xKtzACmAADN+r8oNRgJUbMwAKQOPpQAAUj2ANQ2xNwjKeLOo9AAlMRcRwCWiMjJSeSqTT6YzmciZGyvByZdygA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

If you try and compare the types `Customer` directly, F# won't let you. Feel free to try that.

```fsharp
printf "Customers match: %b" (cust1 = cust2)
```