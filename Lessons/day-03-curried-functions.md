# Day 3: Curried Functions

Yesterday you saw the below function (now presented with signature alongside it):

```fsharp
let sum(x, y) = x + y // int * int -> int
```

It's probably fairly easy to read for you chaps because you are used to seeing parameters provided with this syntax. The more observant of you may have noticed I said that a rule of thumb with commas in F# is that they are generally only used when a tuple is involved, and that is exactly the case here. This type of function is called a Tupled Function.

The ```*``` in the function signature denotes a tuple, hence the name, and in this case it is showing a tuple expecting two ints. The more parameters you add to the function signature, the larger that tuple would grow. For example: ```int * string * long``` translate to a ```(int, string, long)``` in C#

Now let's look at the more widely used syntax for functions in F#, the Curried Function.

```fsharp
let sum x y = x + y // int -> int -> int
```

I'm sure your first thought is "WTF is that!". I admit it does take a bit of getting used to but once you start using it, it becomes second nature pretty quickly. Let's break it down quickly. After let is the function name "sum", there after each parameter is separated by a space "x" and "y", with the equals then denoting the start of the function body where x + y are added, exactly the same as before.

So why would F# developers use a Curried Function over a Tupled Function? The secret lies in the fact that a Curried Function does not need to have all it's arguments provided to it at once, because when you omit an argument a new function is returned instead, with the remaining arguments as inputs, for example:

```fsharp
let sum x y = x + y // int -> int -> int
let addFive = sum 5 // int -> int
let result = addFive 10
printf "%d" result // 15
```

The add5 function has now got the value 5 baked in as the first parameter to the sum function. If you inspect the signature of the add5 function, you'll see that one of the "int ->" is missing, because 5 is now always provided for "x".

If you considered the equivalent in C#, it could look something like this using Local Functions:

```csharp
int Sum(int x, int y) {
    return x + y;
}
int AddFive(int z) {
    return Sum(5, z);
}
var result = AddFive(10);
```

You may or may not see the power in this composition tool yet, but hopefully over the coming weeks it will become more apparent as to why it is used.

On top of that we'll also come to see why the lighter syntax of F# gives more benefits over the C# version above, which at the moment is not too dissimilar. You can of course write the C# version as an expression body using a lambda instead, but generally C# code is imperative and so would require the brackets around it to help the logic flow.

So to recap, Tupled Functions force you to supply all your arguments at once (like C#)
Curried Functions allow you to supply only some of the require arguments, where by a new function is returned with the remaining parameters expected.

[Try today's code](https://try.fsharp.org/#?code=DYUwLgBAzgrgthAHhAnhAvEiBqVED0+EAlgHaQC0AfCeRNbWALABQokAhgCZcBixANxAZo8CAFYCRMpRozW7CACcQsYJEzc+g4QEYADKwAOSmQDMIAIgCkXS8tUx1UiLvFA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)