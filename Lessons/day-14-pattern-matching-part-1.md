# Day 14: Pattern Matching (part 1)

Put simply, pattern matching is if / else on steroids *. C# has had a form of pattern matching for a while, it was very limited at first. In C# 8.0 they gave it a boost in the arm, which you can [read about here](https://docs.microsoft.com/en-us/archive/msdn-magazine/2019/may/csharp-8-0-pattern-matching-in-csharp-8-0) if you are unaware of the new functionality.

\* You remember this reference? Top marks for those that do.

You can use it to assign a value to a variable, or as the last step before returning a value from your expression. The syntax pattern matching is as simple as:

```fsharp
match [variable] with
| [matching pattern] -> [output]
| [as many patterns as required] -> [output]
```

The key point here is to make sure that the branches of logic, start inline or indent further than the match x with syntax. The below example would throw a compiler error at you, try it and see what the compiler says.

```fsharp
let isTrue bln = match bln with
    | true -> "Yes"
    | false -> "No"
```

Let's create a very simple function to show the above

```fsharp
let isTrue bln =
    match bln with
    | true -> "Yes"
    | false -> "No"
printf "%s" (isTrue true)
printf "%s" (isTrue false)
```
[Try the code](https://try.fsharp.org/#?code=DYUwLgBAlgzgKgJwK4ggI2AOwgXgLABQExEAtgIZgDGAFulhAO5Rg2EkQA+EYyqAtAD4IAIgCaIGCPYluAM3LAYA4SIByAe2kFCABwRRMYOaICkUiAApYiFDz4BKPQaMmR5kVZt8ICpSAcgA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

The above is called the Constant pattern, because we are matching the value of the variable against a constant compiled into our branches of logic.

Another useful pattern is called the identifier pattern. You can associate this with your understanding of discriminated unions, where by you state the label of what you are trying to match.

We can do this for an Option, which is a discriminated union of

```fsharp
// Some | None, like so:
let isOptionTrue bln =
    match bln with
    | None -> "Nothing provided"
    | Some true -> "Yes"
    | Some false -> "No"

printf "%s" (isOptionTrue None)
printf "%s" (isOptionTrue (Some true))
printf "%s" (isOptionTrue (Some false))
```
[Try the code](https://try.fsharp.org/#?code=DYUwLgBAlgzg8gBzFA9gOwCoCcCuIIBGwaEAvALABQENEAtgIZgDGAFocRAO5RitW0IAHwgA5dPgC0APggAicXyhoA5hARYUANygATELrkDaIgMoo6+MLimy5ATRAwj1ExHOWIAMwbAYt+XEXKg1lMC95AFJnCAAKWERkdGw8MQkAShCsMIi5aLk4hKRUTBs4jysbdMzKULRwqJj4+GLkstiK719-aqA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

Or to prove the point that it will work for any discriminated union we'll use one of our earlier examples:

```fsharp
type Address = {
    Line1 : string
    Road : string
    City : string
    PostCode : string
    Country : string
}

type PreferredContactMethod =
    | Email of string
    | Mobile of string
    | Letter of Address

let sendMessageToCustomer contactMethod =
    match contactMethod with
    | Email email -> printf "Sending an email to: %s" email
    | Mobile mobNumber -> printf "Sending text to: %s" mobNumber
    | Letter address -> printf "Sending letter to: %s %s, %s" address.Line1 address.Road address.PostCode

sendMessageToCustomer (Email "john@fsharp-city.com")
sendMessageToCustomer (Mobile "0800-fsharp")
sendMessageToCustomer (Letter {
        Line1 = "15"
        Road = "The High Street"
        City = "city"
        PostCode = "N1"
        Country = "England"
    })
```
[Try the code](https://try.fsharp.org/#?code=C4TwDgpgBAggJnAThAziqBeKBvAsAKCiKgBkBLAOwgEYoAuKFYRSgcwOKgCUB7AQzj1GzNh2IBhMqCFMWFdoWIAFHk3E840BrNGKi6gK4VmIGSPkEAvgVCQoS5ADMIiZHHXG+AY2ABZCMAAFhqYYkQAPlAAogC2fGQANlA8jsJyCpyRvjwARonQKWm6maQBwC7JqfBIqCgEBAkBjBAUcP5ofKwQACo84gZMPDEVXjyePv5BIRhhUHHAXoFQo+N+AcGCAO5SgbORsfFJEHGJUAC0AHxQYHLAqQBEAMotcGxQfBRQx4dQwDwMAFIUPcvicEnsoNk8o05rkAHIGGI5CqXa63B7PVpvcoAD2Av3+UCBIJi8MRyMQEJIZQqAhqaHOVxulDuUCeLzejWA5UQBMB6CBABoicD3ghkGgAHTkKi0OkSlCS3gCMX0xUqNQaCD1fAoF7tFCdHp9AZ-Ya8gAUB1O9wAVjxAhQAAKOFCBPiIMBnLxSECS0Yxe4ASgIetaBqNvX6g3NUAtUPybIADAAOJNJs6u92e4Oh-W1SMmmMVC3U7kVPB6TikSg0TBs6gAVnus04ysEWHu3UC0AAEmRWEtHswIAEW1XOJJpJ2faBx9XlKpgOpNPX7nDqPOF1BDMZEKZO1F5AkPnAt0RLEGgA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

The one thing I've not told you about pattern matching is that it must be exhaustive. The same is true in C# too, except that in C# it cheats by making you add a default / discard branch. In F# it can often (not always) work out whether you have covered all options. Let's say the business decides we should be able to contact our users via Skype, now you head back into your code to add the Skype option and try to run the code:

```fsharp
type PreferredContactMethod =
    | Email of string
    | Mobile of string
    | Letter of Address
    | Skype of string
```

You should notice a compiler error telling you that you've not handled the Skype label. Being a good developer you would add that option, and handle the scenario as required.

Sometimes it's possible that you only care about certain scenarios, in those cases it may be tempting to use wildcard pattern and simply ignore anything you've not handled like so:

```fsharp
let sendMessageToCustomer contactMethod =
    match contactMethod with
    | Email email -> printf "Sending an email to: %s" email
    | Mobile mobNumber -> printf "Sending text to: %s" mobNumber
    | Letter address -> printf "Sending letter to: %s %s, %s" address.Line1 address.Road address.PostCode
    | _ -> printf "Don't worry about it"
```

The problem with doing this is that you have now lost the exhaustive pattern matching available to you. If we added another contact method to our list, it would fall into the above and wouldn't warn you that you should handle that. Use the wildcard safely.

There we go, a quick insight into pattern matching. You may be surprises by the concept of the above being a "quick insight" into pattern matching, but it is true. There are many patterns that can be used in pattern matching, and we'll cover more of next time. The above is good enough for today though, and hopefully let the idea settle into your mind of how it can be useful, especially when you are at the end of an expression and you have different branches of logic that you need to apply to provide the output.