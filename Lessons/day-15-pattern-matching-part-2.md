# Day 15: Pattern matching (part 2)

Last time we started our journey on pattern matching. While I am not going to cover everything in pattern matching I would like to continue from where we left off last week and cover some more patterns you are likely to use.

Last week we discussed the constant pattern, which is as it sounds. You are matching against constants:

```fsharp
let isTrue bln =
    match bln with
    | true -> "Yes"
    | false -> "No"
```

And we had the identifier pattern, where by you state a discriminated union label you wish to match against, our example being:

```fsharp
let sendMessageToCustomer contactMethod =
    match contactMethod with
    | Email email -> printf "Sending an email to: %s" email
    | Mobile mobNumber -> printf "Sending text to: %s" mobNumber
    | Letter address -> printf "Sending letter to: %s %s, %s" address.Line1 address.Road address.PostCode
```

In the identifier pattern you'll notice the value we unwrap from the label is assigned to a variable. Assigning the variable is of course very useful as we have it unwrapped to the type we need it to be. This allows the above example to get access to the address type properties.

Another benefit of assigning a variable to what you are matching means that you can apply a _when_ guard:

```fsharp
let rand = new System.Random();
match rand.Next(0, 20) with
| a when a >= 10 -> printf "Higher than 10: %d" a
| a -> printf "Lower than 10: %d" a
```
[Try the code](https://try.fsharp.org/#?code=DYUwLgBATghgdgEwgXgnEB3CBlAngZzBAFsA6AJXgQHtiAKASgG4BYAKHeJjAGMALaFVIA5EAA8wdAAwAaCACYpDCBgCWYPuwA+EGCr4g4uiAD5UARikQAtCYgAHKKrhgAZhABEACVUBzA1AQGvAQlgBcEACkCB662sa2Dk4u7h4AMtQYIIHBRuFRMbpAA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

A word of warning about the when guard. It comes at a cost, because the compiler will _not_ look inside the when guard to work out if all eventualities are handled, and as such the following would tell you that you have not handled all eventualities, even though we know it has been.

```fsharp
let rand = new System.Random();
match rand.Next(0, 20) with
| a when a = 10 -> printf "Higher than 10: %d" a
| a when a <= 10 -> printf "Lower than or equal to 10: %d" a
```
[Try the code](https://try.fsharp.org/#?code=DYUwLgBATghgdgEwgXgnEB3CBlAngZzBAFsA6AJXgQHtiAKASgG4BYAKHeJjAGMALaFVIA5EAA8wdAAwAaCACYpDCBgCWYPuwA+EGCr4g4ulBACMUiAFoAfBAAOUVXDAAzCACIAEqoDmBqBAa8GZSAFwQAKQI7rraxhgGRnoAPKjmVrYOTq4eADLUGCABQUbUASAAjgCuMMCB1CHhUTEwQA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

Record pattern - there is now an equivalent of this in C#, it allows you to match against part of the record:

```fsharp
type Contact = {
    FirstName : string
    LastName : string
}

let whoIsIt person =
    match person with
    | { FirstName = "John" ; LastName = "Harman" } -> printf "John is the best"
    | { LastName = "McEnroe" } -> printf "You cannot be serious!"
    | _ -> printf "Not sure who this is: %A" person

let john = { FirstName = "John" ; LastName = "Harman" }
whoIsIt john

let mcinroe = { FirstName = "John" ; LastName = "McEnroe" }
whoIsIt mcinroe

let imposter = { FirstName = "john" ; LastName = "Harman" }
whoIsIt imposter
```
[Try the code](https://try.fsharp.org/#?code=C4TwDgpgBAwg9gO2AQwMbCgXigbwLABQUxUAYgJYBOAzsAHLIC20AXFLZeQgOaElQAZZLQbMobDl14EAvoUIAbCBgDuACzgBJapoyQaiLHxKNkwVGqj7qhleWBrjxAD64yVEU2jYARACk4NQQfKABuQWF6LywoHwAJZEpTYKgZKABaAD4rTiQAM1iAoKhyaigHaAAjCFofJyhXHAjPMV8AWVQAUQRKOAgQtKycrmACnwBNOABXKFRkBAQ4DGr2CE5p6gBCOqISVwB9DOywXNHYuiX2KcpodThytVKS6jYAUgBBEOtDeQIlDAAVoEEDEmhQaFFWoVgSFwkIWt5YgkkvMBoQ7tpdFAgUFfv8oIxUFxeoiwR5IYj-DCws0KTEfB1uiS0QQMToMITiX08coSowwHBaGtQe4IaJKTiUnDIuL6cjkiy2VjyPzBcA1oQgA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

NULL pattern - Yes the dreaded `null`. Only really necessary when dealing with BCL libraries, rather than F# libraries. Useful none the less.

```fsharp
let isItNull (str : string) =
    match str with
    | null -> printf "Is a null"
    | a -> printf "%s" a

isItNull null
isItNull "This ain't no null"
```
[Try the code](https://try.fsharp.org/#?code=DYUwLgBAlgzgkmAcgV2MCAKGYBOEBcE2OUAdgOYCUEAvALABQEzEAtgIZgDGAFkbhADuUMD0YsIAHwilU6ALQA+CAAcSpMADMIAIjgwI7GXJ3iW0o0tXqtugKQwdhxo1gIUaY2lfwkc3QAqPLCGZADkkKQA9l7AOkA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

One last one for the day, the Tuple pattern. Pattern matching only allows for a single input, however we can get around this with a tuple. Simply supply the values you wish to match with a comma between them, and in the patterns be sure to supply one match per comma.

```fsharp
let optionEqualOrGreaterThan a b =
    match a, b with
    | Some x, Some y when x >= y -> printf "true"
    | Some _, None -> printf "false: left some, right none"
    | None, Some _ -> printf "false: left none, right some"
    | _ -> printf "false"

optionEqualOrGreaterThan None None
optionEqualOrGreaterThan (Some 1) None
optionEqualOrGreaterThan None (Some 1)
optionEqualOrGreaterThan (Some 1) (Some 1)
```
[Try the code](https://try.fsharp.org/#?code=DYUwLgBA9gDmCWUB2BRAjgVwIbAPICcBxfELMEfAFQAsskIsIAjCAXgFgAoCHiAWzIBjagwA0zCAHd4Yal14QAPhADKUPiAgAPcWo0QAnlOoh6WiAD5WhiAFoLEGPnhIwAMwgAiMPgwhP8rzKepoA+uIAcsia9o7Orh6ebjgAziAAXBCgbpAp6iDizgDm1JBI0QHcQRBRSAWq+RChdg5OLu5eycBpmdll0YXwJbn5lQrKzbFtCZ2p-lxcsAjI6Nh4RCRkFDR0NdF7dYtwiKiYOATEpORUtPQAFCEQAIwAlAcgR8unaxeb1zv0WqaB6NV6fE6rc4bK7bW4QEH6V7wx6vIA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

There's more going on in the above, so we'll break it down piece by piece. We are accepting two option values of int, and if they are both Some then we can compare then inside the when guard and print true to the screen. I then over engineer the scenario to show off the ability to check if either side is Some with a None, before finally outputting false.

oh, I forgot to mention that F# knows that both parameters are the option type, because in the pattern matching we've used the Some and None labels. Once the values are unwraped it can see we're doing a comparison using greater than, which by default it considers to be an integer.