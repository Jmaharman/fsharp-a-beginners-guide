# Day 21: Type aliases
Type aliases allow you to alias, go figure, an existing type. We do not create a new type, it's merely a shortcut to the existing type.
The syntax looks like this:

```fsharp
type [typename] = [existingType]
// e.g.
type CustomerId = string
```

C# can do aliases too, but you have to declare them in every file you would like to use them, which makes them harder to use. In F# you can re-use them as often as you like.

```csharp
using CustomerId = System.String;
```

Why is this helpful? A few reasons really. Take the below example:

```fsharp
type CubeDimensions = double * double * double
type Package = {
    Dimensions : CubeDimensions
}
let dimensionsTuple = 12.1, 10.5, 100.5
let package = {
    Dimensions = dimensionsTuple
}
```
[Try the code](https://try.fsharp.org/#?code=C4TwDgpgBAwgrgIwgEQJYFsIDsDOqD2uUAvFACb6IA20AVOZQjVPRdRALABQ3okUABQCGAYwDWQgObRSAb25RFUNJlwEiALliIUGbHkI5uAX27cawcnrWGAKnDDNSARgBMAOmcAaKM4AM7gCsPv4BgWZcFlBgohLSJFDyXErK1gZEpGRp6jj2jpxcpjxcQA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

Here we've not created any special constraints around the cube dimensions, but it's made the Package type a little more readable. You only need to look into what `CubeDimensions` are if you are going to interact with them, at which point you can mouse over and see what the definition is. The alternative is reading through the code and taking a second to realise that the tuple3 of double is a representation of the cube's dimensions.

To prove there are no new types involved here, you'll notice that `dimensionsTuple` is created as a standard tuple, it doesn't reference `CubeDimensions` at all. It's perfectly valid to assign that to the `Dimensions` property.

Being lazy devs, when we find a long generic it can be a pain to type these values out and being C# we have to declare these types everywhere. While F# doesn't require us to always define our types up front, sometimes it can help. Type aliases can save us some keystrokes in these situations too. Imagine you are communicating to a remote server:

```fsharp
type ServerError =
    | ValidationError of string list
    | AuthError of string
    | Exception of string
type ServerResponse<'a> = Async<Result<'a, ServerError>>
```

'a is the type you are expecting in response, and the DU is a list of possible `ServerErrors`. The request would be `Async`, and your response from the server would either be `Result.Ok` (`'a`, the left generic) or `Result.Error` (`ServerError`, the right generic).

Using ```ServerResponse<MyType>``` is far simpler to consider, than the full type of ```Async<Result<'a, ServerError>>```. When reading your code you can take it at face value that you will get the value you are after, but you can also dive into what else may go wrong.

While it is possible to create simple aliases in C#, you cannot create typed aliases because the whine at you.

```csharp
using CustomList<T> = System.Collections.Generic.List<string>;
```

Even if you could it still wouldn't be as useful, because as we said above, you have to copy those aliases into every file where you use them.