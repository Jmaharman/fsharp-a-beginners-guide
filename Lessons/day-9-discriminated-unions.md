# Day 9: Descriminated Unions

A descriminated Union has no direct comparison in C#. At a stretch you could say it's like an enum, or similar to an abstract class with it's various implementations as the various options.

Neither are ideal as they have problems of their own. For example, any ```int``` wrapped in the ```Enum``` type is valid, even if you only have 3 options that you have defined. The same could be said of an abstract class, you may create only the implementations you consider valid, however another developer could add another implementation and may not handle this new implementation everywhere it is required. Not to mention how verbose creating an abstract class, and it's inheritors is.

Discriminated Unions can solve both of the above problems in a simple to use syntax.
The syntax is fairly simple, you state the name of the discriminated union, and then a pipe ```|``` character followed by each item's label.

```
type PreferredContactMethod =
    | Email
    | Mobile
    | Letter
```

The above example is similar to an Enum, except the only possible values for that union are the ones I have defined. That's not all though:

```
type PreferredContactMethod =
    | Email of string
    | Mobile of string
    | Letter of string
```

This time we've stated that each of our labels should hold data ```of``` a string type. One way of thinking about the label is as if it is a wrapper to the data. The label isn't the data itself, it merely acts as a way of discriminating between the various labels available on this union. Once you know the label, then you know the data within it.

You can place any types you wish inside the label, for example our Address Record Type from yesterday can be used to store the address details for the letter.

```
type PreferredContactMethod =
    | Email of string
    | Mobile of string
    | Letter of Address
```

The flip side of that is that you can use a Discriminated Union inside a record type too:

```
type Customer = {
    FirstName : string
    LastName : string
    ContactMethod : PreferredContactMethod
}
```

Discriminated unions are no different to Record Types with regards to equality, we can compare two different variables in exactly the same way as before:

```
let preferEmail = PreferredContactMethod.Email "john@johnharman.co.uk"
let preferEmailDuplicate = PreferredContactMethod.Email "john@johnharman.co.uk"

printf "Are these unions equal? %b" (preferEmail = preferEmailDuplicate)

let customer = {
    FirstName = "John"
    LastName = "Harman"
    ContactMethod = preferEmail
}
let customerDuplicate = {
    FirstName = "John"
    LastName = "Harman"
    ContactMethod = preferEmailDuplicate
}

printf "Are these customers equal? %b" (customer = customerDuplicate)
```
[Try the code](https://try.fsharp.org/#?code=C4TwDgpgBAggJnAThAziqBeKBvAsAKCiKgBkBLAOwgEYoAuKFYRSgcwOKgCUB7AQzj1GzNh2IBhMqCFMWFdoWIAFHk3E840BrNGKi6gK4VmIGSPkEAvgQKhIUJcgBmERMjjrjfAMbAAshDAABYamGJEAD5QAKIAtnxkADZQPE7CcgqcUX48AEZJ0KnpulmkgcCuKWnwSKgoNvh20OIGTDyxlVh4elAAYmSITAByfB1mGeGkfMOjWsUWPZ7APv6BIYIMjhAubhAePF6+AcEaVg2JgVBgzq5xCclYWzvuSyvH6wB0d0lQAEQAVjwghQAAKA4FBPiIeIUD7eHgfAwAa1+BAuwCuN0Q30SABEDGBEmRvHwKpgHFiXgdlkc1hovvEfgCgaDwRRIdC+LD4YiUQ1rpRgGlfjBkFBgqhoEYyAd0BAAI4GPiJAD8UAApLlflAABTXba3RkPTEG7FG-GE4mkiAASnOl28rWA7U6OEm-UGwBGYywvwAUizUT0SNMvbNyb8ABJQmFBzivWknQRYfU7HFnfDoqCOtodRAWokksldd0DGY+v4B4Fx4gh8vQX3RzkUGv6alvOnJk1p80EwvWjMEAXGYWi6ASlDQHPOvNyxXKtWa7U66cuxDk1d5gtWio2oA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

So what would happen if we tried to compare the simple type value of email, a string, with the descriminated union value? It would fail.

```
let email = "john@johnharman.co.uk"
let preferEmail = PreferredContactMethod.Email email

printf "%b" (email = preferEmail)
```
[Try the code](https://try.fsharp.org/#?code=C4TwDgpgBAggJnAThAziqBeKBvAsAKCiKgBkBLAOwgEYoAuKFYRSgcwOKgCUB7AQzj1GzNh2IBhMqCFMWFdoWIAFHk3E840BrNGKi6gK4VmIGSPkEAvgQKhIUJcgBmERMjjrjfAMbAAshDAABYamGJEAD5QAKIAtnxkADZQPE7CcgqcUX48AEZJ0KnpulmkgcCuKWnwSKgoNviJgVAQ8UmYUABEAFY8QRQAAr39QXyI8RQAdN48kwYA1p0ETcBQYM6ucQnJWI4QLm4QHjxevgHBGpNb7a3bDeuUwGmdAKS5nVAAFLftWOv7mzaiQAlAQgA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

The reason for that is because everything in F# is explicit. The F# compiler can infer which types you mean, but it will not cross that line into considering two different types, inherited or otherwise, as the same. This may feel like a step backwards compared to C#, but ultimately that is how F# is able to infer the types that you are using. A type can mean one thing, and one thing only. In C# you can state one type and the C# compiler will happily infer the types for you at the appropriate time, for example an concrete implementation of an interface.

One way to handle the above comparison would be to put them both in the same union type and then compare. The alternative is to use pattern matching but we'll save that for another day.

Everything I've done up until now always includes the discriminated union name, as well as the label. It turns out F# only needs that discriminated union name if the labels clash with other labels / types. Otherwise it knows the label belongs to the union, and as such you don't need the union's name at all:

```
let preferEmail = Email "john@johnharman.co.uk"
```
[Try the code](https://try.fsharp.org/#?code=C4TwDgpgBAggJnAThAziqBeKBvAsAKCiKgBkBLAOwgEYoAuKFYRSgcwOKgCUB7AQzj1GzNh2IBhMqCFMWFdoWIAFHk3E840BrNGKi6gK4VmIGSPkEAvgQKhIUJcgBmERMjjrjfAMbAAshDAABYamGJEAD5QAKIAtnxkADZQPE7CcgqcUX48AEZJ0KnpulmkgcCuKWnwSKgoNvh20OIGTDyxlVh4elAAYmSITAByfB1mGeGkfMOjWsUWPZ7APv6BIYIMjhAubhAePF6+AcEaVg2JgVBgzq5xCclYd0lQAEQAVjxBFAACH19BfEQ8QoADpvDwQQYANYvAgXYBXG6IJ6JAAiBjAiTI3j4FUwMXiz3enx+fwoAKBfFB4MhMIa10owDSLxgyCgwVQ0CMZAO6AgAEcDHxEgB+KAAUlyLygAAprttboSHoiFcilejMdjcRAAJTnS7eVrAdqdHCTfqDYAjMZYF4AKRJsJ6JGmVtm+JeAAlAcCnZwlitjut8fKdiizvh4VBDW0OogNVicXiuuaBjMba8HV8-cQXenoLbvZSKDn9Adlkc1qEsKHFfcE1qKhGCAzjMzWdAOShoDHjXG+YLhWLJdKZb2TYh8eO4w2k7qgA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

It's fair to say that a discriminated union is the OR to the record types AND, again described as such by Scott Wlaschin. Combining the two together can give you a very powerful way of expressing what your model should contain, without the need to explain verbally or via comments what should or shouldn't be set in X scenario. This allows us to avoid nulls and thus reduce possible bugs in our code.