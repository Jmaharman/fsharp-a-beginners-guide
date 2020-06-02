# Day 7: F# Compiler "magic" and Even More Functions!!

Up until now we've managed to avoid specifying any types in our F# code. I imagine that makes you feel uneasy, after all you are used to seeing what the function has as inputs and outputs as a glance.
That being said, people also felt uneasy when C# first introduced the var keyword:

```csharp
// Explicit variable type, the go to
int age = 20;
// To inferred type, what sort of horrible-ness is this!!
var age = 20;
```

The F# compiler is like the above, but on steroids*. While that is an overly simplistic comparison, it feels pretty apt.

\* I have to chuckle when saying that, because the "Get Programming with F#" book said that many F# features were like X on steroids. *A lot of examples from this lesson are taken from Isaac's book, I'd recommend reading it to get a more verbose explanation of this topic.*

Your code will often drop hints as to what you are expecting based on what your functions are doing. For example:

```fsharp
let getLength name = name |> String.length
```

The fact that String.length expects a string means that name should be a string.
The above F# book gives a very good example of code that doesn't give the compiler enough information to infer the type of name:

```fsharp
let getLength name = sprintf "Name is %d letters." name.Length
```

Here F# only knows that the name variable has a Length property, but that's not enough to isolate the type in use.

Sometimes we do need define your parameter types up front, this common for classes that come out of the BCL, but it can also be true for your own types if they happen to overlap heavily.

To resolve the compiler error above you can specify the type as such, by wrapping them in brackets and stating the type.

```fsharp
let getLength (name : string) = sprintf "Name is %d letters." name.Length
```

Previously I've mentioned that F# can infer that any parameter either side of the + operator should be an int, but we all know that you can + many types including floats / decimals and even custom types that implement the operator. In this case though, F# has decided that the default type for the + operator is an int.

If you actually want to define a function for a float, you would do it as such:

```fsharp
let sumFloat (a : float) b = a + b // float -> float -> float

printf "%f" (sumFloat 1.2 1.2)
```
[Try the code](https://try.fsharp.org/#?code=DYUwLgBAzgrgtgMWAewIaQBSogLggMxXQEoIAjCAXgmwGpyBYAKGYAcAnASwDsx8IARAFJ8AiBliIikAIwA6AEwR5C4kA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

Notice only the first parameter required wrapping, it could then infer that the rest would also need to be a float.

Inference has it's obvious benefits, but there are even more powerful benefits you won't wouldn't have even thought about until the compiler does it for you.

```fsharp
let concat item1 item2 = [ item1 ; item2 ] // 'a -> 'a -> List<'a>
```

Above I've declared a function that expects 2 parameters and returns them in a list*. Looking at the signature you'll see it's decided that the inputs and the output list are a generic type, very cool!

\* The angle bracket syntax in F# is for a list not an array, it is NOT the same list type that you are used to in C#, because it's immutable as well as other properties. We'll discuss collections in more detail another day.

The above may seem obvious, but it does this with non trivial examples too. :siren: This code is pointless, but it shows generics being applied again :siren:

```fsharp
let arbritraryFunction map item1 item2 = map item1 item2 // ('a -> 'b -> 'c) -> 'a -> 'b -> 'c
```

_You may find this example complicated while you are still learning how to read functional signatures, my apologies if it's too hard to follow but I am deliberately trying to make this example a little harder to demonstrate the feature._

F# has looked through the function body and worked out that the map parameter is a function, and that it is expecting two inputs, which it defines as types 'a and 'b. It doesn't know what map would return, but it doesn't need to because it can apply an additional generic parameter to it as 'c. This then means that item1 must be of type 'a, and item2 must be of type 'b. 🤯

When using that implementation 'a, 'b and 'c could all be of the same time of course, but they could be completely different types. It all depends on how you use it.

Perhaps you would find the Tupled version of the same function easier to read (notice the slight difference in the function signature, hint: *)

```fsharp
let arbritraryFunction func item1 item2 = func(item1, item2) // ('a * 'b -> 'c) -> 'a -> 'b -> 'c
```

When your code isn't compiling as you'd expected, it can be daunting at first. In functional languages they often say to "follow the types". What does that mean? Essentially take your time and think through the functional signatures, check what you are expecting and what the compiler says it is expecting. I often find similar issues in C# when working with inferred generics especially with abstracts and implementations.

Here's another example ripped straight out the book, you'll need to look at this next set of code in the try.fsharp.org tool so you can see the compiler error

```fsharp
let sayHello(someValue) =
    let innerFunction(number) =
        if number > "hello" then "Isaac"
        elif number > 20 then "Fred"
        else "Sara"
    let resultOfInner =
        if someValue < 10.0 then innerFunction(5)
        else innerFunction(15)
    "Hello " + resultOfInner
let result = sayHello(10.5)
```
[Try the Code](https://try.fsharp.org/#?code=DYUwLgBAzghgngCRMYB7AFFVBbEA1GYAVxAEoIBeAWACgJ6JRIBLAO1ZACcAxI1gYzDNUrdKyLYARl3LU6DBcwBmEcVK4QAfBABEAC2RodEMAda6AkrBj8dtBQuTLVE6Zy0QATAAYTZ3dycIAAmdvIOEMhQILoAyjCcMGH2DEwQQVBEwGAA8koW7BpyEfTOWLgExDEAPBAAjN4AdL6mIOZsHDx8gsKiAKykKRFRMR1cvAJCIuh1A7RDukgoqLoQANTpIJnZeQWd8zRpGVmQFNDwS2gzTQNAA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

You'll notice the compiler has 3 different warnings, but the problem is actually higher up in the function. If you hover over innerFunction (on try.fsharp.org) you'll see that it's expecting a string, not an int. That's because we're doing a comparison with a string in the first line, so it then assumes everything else is wrong.

If something like this is confusing you, and this applies to C# generic types too, a helpful thing to do is to explicitly state the types you are expecting. In this case if you updated the innerFunction definition to:
let innerFunction(number : int)

You should then see that the actual issue being highlighted correctly. Correcting that if statement so that it correctly compares to a number (for example, 10) then means everything can compile, and you can happily remove the type from the function definition should you wish.