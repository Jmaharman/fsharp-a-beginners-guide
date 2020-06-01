# Day 11: Single Case Unions

In our example of Address last week we had a type with each string representing different parts of it:

```fsharp
type Address = {
    Line1 : string
    City : string
    PostCode : string
    Country : string
}
```

As a type gets larger it's fairly easy to create bugs in our code by assigning the wrong variable to the wrong property, they are both strings after all. It's easily done, often when copying and pasting. Another common scenario is when accepting a Entity Id as a function input, or even easier to get wrong is a boolean in a signature call. Make the wrong tweak and you could have the wrong boolean being assigned to the wrong argument.

Single case unions to the rescue!

The syntax is very similar to a discriminated Union, however instead of using a pipe character to separate the OR options, you only give it one. As follows:

```fsharp
type Country = Country of string
```

They act in a very similar way to discriminated unions too because the label once again acts as a wrapper for your data. Like so:

```fsharp
let england = Country "England"
```

To get the value out again can be done in a few ways.

```fsharp
let (Country englandStr) = england
printf "%A" england
printf "%s" englandStr
```
[Try the code](https://try.fsharp.org/#?code=C4TwDgpgBAwg9gVwHbAE4igXlol6pwBmUAzmgJZIDmAsAFD0A2EwUE1jAhkgCZY7I0GAEQBRDtx7D6TFlAAU8QfnZUuvAMpoAlP1XqeMumFSVgxYQFIAgsLYTe9E2YuWSd-ZK2ogA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

We could create a module to help define a function to unwrap it for us, which can be cleaner at times

```fsharp
module Country =
    let value (Country c) = c
let england = Country "England"
printf "%s" (Country.value england)
```
[Try the code](https://try.fsharp.org/#?code=C4TwDgpgBAwg9gVwHbAE4igXlol6pwBmUAzmgJZIDmAsAFD0C2cAJggDbTzJoab1RBUTsCgA3AIbsE0ABTc8GAMYBKLFCX16IqBGrsJSFuoW8oAIgCi+wy3Na6YVJWDFzAUhLmo83LwB0ktLQelQGRipAA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

You can also use pattern matching too, but that's a bit contrived for a simple scenario like this.

As with discriminated unions you cannot compare a single union case with a variable of it's inner data, you would have to wrap or unwrap it first

```fsharp
let england = Country "England"
printf "%b" (england = "England") // Compiler Error
let englandStr = Country.value england
printf "%b" (englandStr = "England")
```
[Try the code](https://try.fsharp.org/#?code=C4TwDgpgBAwg9gVwHbAE4igXlol6pwBmUAzmgJZIDmAsAFD0C2cAJggDbTzJoab1RBUTsCgA3AIbsE0ABTc8GAMYBKLFCX16IqBGrsJSFuoW8oAIgCi+wy3P0wqSsGLmApACNzUWXqoGjdSsbI3M1AHpwnEYwck5UKEtUVDhULTodPwCWAGU0E1xeADpJaWgs2wcnFFdPb18Q3PzsYP9bMKA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

It goes without saying that equality is structural once more:

```fsharp
let england = Country "England"
let england2 = Country "England"
printf "%b" (england = england2)
```

Let's update our Address example to use single case unions:

```fsharp
type Line1 = Line1 of string
type City = City of string
type PostCode = PostCode of string
type Country = Country of string

type Address = {
    Line1 : Line1
    City : City
    PostCode : PostCode
    Country : Country
}

// When creating the address we must pass the string value into our SU
// Feel free to play around with swapping the SUs to see a compilation error
let address = {
    Line1 = Line1 "221b Baker St"
    City = City "London"
    PostCode = PostCode "NW1 6XE"
    Country = Country "UK"
}

let newAddress = {
    address with
        City = address.Country // Won't compile because City is not compat with Country
    }
```
[Try the code](https://try.fsharp.org/#?code=C4TwDgpgBAMglgOwgRigXlolUD2AzKAZ2ACdEBzAWAChRIoBhOUdR5kXA4shK28aAAUcxBjgAm0DMNETo+IqQo060MQFcEpDhg1aSHBd2XUVAqAEFx4khEKFWAbxpRXmJKgBc7lC7dMWbwCQP1cZYDFJKG9wyIhQxhxNbWjE5IMaAF8aGgB6XKgAdQALCAQoAGNbAENgCihgUqhq61t7KAB3aABbdWIoMGr2xuhjXigAN2qAG3VoRGAcXHUSKABlAFU8goAxCAhpqDxbaEWB6eqOapIkhHFO5mKiDuqwMHqR9Y2HM8J95sqOG67wudRw5QgJBuJBo0wgwGarTsDgwzmobh8qAw8A8UAARAAmAnIABGUAAQtUANaQ9bAPEJYKsJl4mDg8TghnotyxOSsXlRPEAOUKqAAbAANACiXIxehSuluKTxGwA0lzsqZqHCEUgOlYbMinAkWob2h1HlAEnL2KxTW1CAA6eUGKD5IrggDkCIqQJB0BJEAq1T6altcAcCBwPr9tQejTS+hC3NcmSAA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

This may feel over the top, but it is so quick and easy to do that it has few downsides. You can use it at your own discretion. If you do though, you've now made it far less likely that you will put the wrong data in the wrong place.

In fact this very simple step can remove the need for a number of unit tests that you may write to ensure the right properties are being placed in the correct fields or passed in as the correct arguments. The fact is that any unit test that does this is often a waste of developer time.

We're not done with SU yet though, we have some more ground to cover with them tomorrow.