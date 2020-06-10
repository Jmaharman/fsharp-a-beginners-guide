# Day 19: Active Patterns (Pattern Matching part 4)

Let's look at a more common Active Pattern, similar to the the Single-case Active Pattern but with a twist. The Single-case Partial is very similar to the Single-case Pattern, except that it allows for the scenario where you can't transform the input to the output. Hence it has the signature of ```'T -> Option<'A>```

A good example would be converting a string to an integer:

```fsharp
let (|Int|_|) (input: string) =
    match System.Int32.TryParse input with
    | true, value -> Some value
    | _ -> None
```

An obvious difference when compared to the Single-case Pattern is the _ in the label list. The label between the first two pipes represents the `Some` outcome, and the `_` represents the `None` outcome. Within the function body you then use `Some` and `None` to provide the two outcomes.

When you come to use the Single-case partial in a match you do not need to worry about stating `Some` / `None` yourself. F# unwraps this for you. If it is Some the label will match and you will be expected to use the variable. For the none scenario a match is not found, and you are required to handle the match through another branch:

```fsharp
let printInt input =
    match input with
    | Int i -> printf "%d" i
    | _ -> printf "`%s` is not valid integer" input
printInt "99"
printInt "99.9"
```
[Try the code](https://try.fsharp.org/#?code=DYUwLgBAFAPgkgOzDA+jAlNAlggDgVzAC4IBnMAJxwHNMBeAWACgJW3WBbAQzAGMALCAGUAnuRAcAdIjABmAEySAKhREAFLhVIgIOApADuWMP2bt2MCJXwgANBABuXYDYgBaAHzCA9hx1OXEDNzVksUdy8AOW8EIKZmUEhcKiQZXTxCCEYWNm4+QT1MoxNg0Ig0rAiIZJwwADMIACIAUgATRt1SiDCqmqQGxoADZtJB3VIIBG9IAKxW9LAQahAKDsKwZmY+sDTGgE49xq2UnaQmg8lDoA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

As you can see we make use of our Int active pattern to get the i variable, then print that value out. When Int isn't matched we use a wildcard to decide what to do instead.

Feel free to play around with the above. It's good to remember that although I showed you the pattern matching techniques one at a time, but they can be combined in many ways. Here's a few examples of using the active pattern with some alternative pattern matching techniques:

```fsharp
let printInt input =
    match input with
    | Int 42 -> printf "The answer to everything" // Constant pattern
    | Int _ -> printf "All I know is that it's a number" // Discard the output
    | _ -> printf "`%s` is not valid integer" input
```

We can run Single-case partial active patterns one after another, so we'll write a few more Single-case partial patterns and use them one after another. One thing that you'll probably notice is that even though you can't see the implementation, you immediately understand what the below is doing. You can see the function definitions via the link.

```fsharp
let printType input =
    match input with
    | Int i -> printf "Int: %d" i
    | Decimal d -> printf "Decimal: %f" d
    | Guid g -> printf "Guid: %A" g
    | Bool b -> printf "Bool: %b" b
    | _ -> printf "Cannot parse into known type `%s`" input
```
[Try the code](https://try.fsharp.org/#?code=DYUwLgBAFAPgkgOzDA+jAlNAlggDgVzAC4IBnMAJxwHNMBeAWACgJWIBbAQzAGMALCAGUAnuRDsAdIjABmAEwSAKhWEAFThVIgIOApADuWMH2ZsIMCJXwgANBABunYNYgBaAHxCA9u22PnIKZsFihungByXgiBTMygkLAAIiA8WFzAqBjYeIQk5FQItBCMLGxcvAIiYpLJqelKKuqa2rqEEIbGQawWVrYOTi4e3r79AV3mEKFDkdHMceDQMADi+FgAJpmYUK3EZJQ09OPl-EKiYOISK+sNahpaOjkGRial3ZYU1nb+g56CPn4DGJmEJhCAzGLzBIwABCXi8GTQWx2eX2hUOrw43BOOyUXgAMl59CAKFBMB0XsDoAAiACMVJgVN6VMwoL+I164wsUCpAAZ6VSAGZOLTM1n-CBC4BaTkQKlUsXsj5A4KTUHguZMeIQXAFMCKYS4FqPYpHLECHbtZ4y6Q6UE6nBgAWy6QkACka3lWBltTSTggaztuqdVJ96TdAvlaxlVwD1EDDuDMbdAEF5dQZbD4RAAEbxpDBzPAN3Z+XZmVTTz2-OygDCnAQCC8kFwdyNYC8EAA1o39AhLAbtAADV2kQeex4aqt6geygCcs6pzCn+sNc9nEgXS91K+0VM4PBkAHY5CAZDxXHIeMfXAAWABsa3PnDvApvrlnaxpApSd4ArCfszpLcHR3WUmWApBQKpABVBAsB4LwKD7AAJRCECpIA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

Now onto the Paramaterized partial. It has the same purpose as the Single-case Partial Pattern, the difference this time is that we can provide it with one or more parameters. This is hard to represent as a single functional signature, so I'll show you a few examples

```fsharp
'T -> 'P -> Option<'A> // Single additional parameter
'T -> 'P1 -> 'P2 -> Option<'A> // Two additional parameters
'T -> 'P1 -> 'P2 -> 'P3 -> Option<'A> // Three additional parameters
'T -> 'P1 ... 'PN -> Option<'A> // The ... represents any number of parameters
```

Let's re-use our Int Single-case Partial pattern, and create a Parameterized Partial.

```fsharp
let (|IntGreaterThan|_|) i input =
    match input with
    | Int value when value > i -> Some value
    | _ -> None
```
[Try the code](https://try.fsharp.org/#?code=DYUwLgBAFAPgkgOzDA+jAlNAlggDgVzAC4IBnMAJxwHNMBeAWACgJWIBbAQzAGMALCAGUAnuRDsAdIjABmAEwSAKhWEAFThVIgIOApADuWMH2ZsIMCJXwgANBABunYNYgBaAHxCA9u22PnIKZsFihungByXgiBTMygkLDSAOIUINwgFIp8nAioGDo6eIQQjCxsXLwCusWGxkGsFtIOTi76fCAIzQEQnlhh3r5d1vXmEKEeEJHRzHHgEHxe+gAyGtTa1ZClZhX8hXoQtSZlDRDJqemZ2Z1yAAwFE7hUSABmEABEAKQAJjqkENTnMAZSxXCC3N46EaNJApNJAy45CAARjufQeTzAr0+PywfwBcOBxkRKIhWChY36jxwmPecEg7Cw1D4kAARtpEdS7CzikYAOR-L4gZ44IwgYDCCAILyQfEXEHEm5vGZMBbLVbaN4ATk1SpVixWFDW7yROuYqoNRreAFUEFgeF4KJ0ABIOhBvIA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

I think the above is fairly self explanatory, it's not a big leap from where we were before. You may not have noticed but we dropped the type definition from the input, that was possible because we are using the Int pattern, which states the type and thus the compiler can now infer what we're expecting.

Finally, we have the Multiple-case pattern. As you have probably guessed, this pattern allows you to define a finite list of labels that the pattern will return. It works in a very similar way to matching on a Discriminated Union. Once you start to pattern match on one of the Multiple-case labels, the compiler will remind you whenever you have not taken care of all the labels.

I'll re-use our previous type Single-case partial patterns and create a Multiple-case pattern, which I've placed in a module to help avoid the clashing of pattern names:

```fsharp
module Param =
    let (|Int|Decimal|Guid|Bool|String|) input =
        match input with
        | Int i -> Int i
        | Decimal d -> Decimal d
        | Guid g -> Guid g
        | Bool b -> Bool b
        | _ -> String input
let parameterType input =
    match input with
    | Param.Int i -> printf "Int: %d" i
    | Param.Decimal d -> printf "Decimal: %f" d
    | Param.Guid g -> printf "Guid: %A" g
    | Param.Bool b -> printf "Bool: %b" b
    | Param.String str -> printf "String: %s" str
```
[Try the code](https://try.fsharp.org/#?code=DYUwLgBAFAPgkgOzDA+jAlNAlggDgVzAC4IBnMAJxwHNMBeAWACgJWIBbAQzAGMALCAGUAnuRDsAdIjABmAEwSAKhWEAFThVIgIOApADuWMH2ZsIMCJXwgANBABunYNYgBaAHxCA9u22PnIKZsFihungByXgiBTMygkLAAIiA8WFzAqBjYeIQk5FQItBCMLGxcvAIiYpLJqelKKuqa2rqEEIbGQawWVrYOTi4e3r79AV3mEKFDkdHMceDQMADi+FgAJpmYUK3EZJQ09OPl-EKiYOISK+sNahpaOjkGRial3ZYU1nb+g56CPn4DGJmEJhCAzGLzBIwABCXi8GTQWx2eX2hUOrw43BOOyUXgAMl59CAKFBMB0XsDoAAiACMVJgVN6VMwoL+I164wsUCpAAZ6VSAGZOLTM1n-CBC4BaTkQKlUsXsj5A4KTUHguZMdheNb4UAQJqcdjFcbxRbSGC1NJOZarDaw+EwQSo6hZHbGjFmY4CN3k8aU6Q6UEBrB+lWW9IQNag8NOSOht5XKPUUGJiDUeMTe3ACAAI1BWdzGZBQydBWTOw1ptwGkN4GJimEuBaj3dnqx3pbvoxFgNkmDoNwBTAAtl0hIAFI1vKQ939TWaikrdmo0NBzhh7KY8AJwL5WsZb3Lra0wOhyOqYmJwBBeXp2eHgt51dn2VZic5+U5g-ziSlmh7ChT3Xc8-0KCdSHlfINWrCha3OCgGybWUAE5kKpZgYLg+tG20KlUIkNCMPnOsEJw2VOB4GQAHY5BAGQeFcOQeBo1wABYADY1gYzh2IFVjXGQtYaQFFJ2IAVlonM6SI2DfHgxDcKZGSsNIpCqQAVQQLAeC8CgEAgAAJXSECpIA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

If you jump into the example you'll see the function being run, and the printed values and their types. If you removed anyone of the matching lines from the parameterType function, you'll notice that the compiler will warn you. Hopefully you remember that this is the same behaviour as when pattern matching a discriminated union.

I'm not sure what your thoughts on active patterns are, perhaps you see their power or maybe like me you think they are clever but can't yet see a use. You may find them hard to read at first, but keep practicing and that will come.

The fact is that they are an invaluable tool. It may take a few times of looking at them and going back to them a few times after that before realising, but keep at it because it is worth the effort.

Today concludes what I would consider the building blocks F#. You haven't been shown everything in the subjects that we have covered, but I think you've seen enough to understand most of the syntax you would see when reading most F# code.

Hopefully the last four weeks have opened your eyes to some benefits that F# brings.