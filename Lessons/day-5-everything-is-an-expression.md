# Day 5: Everything is an expression in F#

Let's define an expression out loud so that everyone is clear. An expression is a combination of values and functions to create a new value, i.e. your code will always return something.
We can obviously write expressions in C#, we've done many in this channel over the last 5 days. Just roll out the lambda syntax, avoid the braces and away you go expression ahoy:

```
Func<string, string> concat = (a, b) => a + b
```

What C# also lets you write is Statements. A statement is when you have code that will execute but return nothing. Notably a MethodGroup returning void, or an Action.

```
Action<string> log => msg => Console.WriteLine(msg)
```

The question is, if F# only allows you to write expressions, how do you write a function that would be void in C#?
Answer: It doesn't, because everything is an expression and therefore it always returns something. I'm sure there is some deeper meaningful reason in the mathematics that F# is built upon as to why this is, but in laymen's terms it comes down to compossibility.

How can you chain two functions together that return nothing in an expression? You can't, it's impossible. So what do we do? The answer is Unit.

Unit acts as a placeholder when no other value exists or is needed.

In fact each of us has used a Unit at some point already in our C# code because that is a type that Mediatr uses when you have execute a command that returns no value. While it may display this to us as a handler that returns void, under the hood that then defers to a handler that returns a Unit. The reason they did this was to simplify their own codebase.

Note that Unit does not exist in the BCL or C#, it is a F# type. Mediator implemented their own Unit-like type.

Here is the equivalent to the above C# code, we have to explicitly return unit, which in F# is represented in code as open and close brackets:

```
let log msg = printf "%s" msg ; () // string -> unit
log "Hey there"
```

Great, so we know that a function must have an output, and the lowest common denominator for that in F# is a unit.

The next question is how can you compose something that has zero parameters. For example:

```
let doNothing() = ()
```

You may be able to guess once I give you the signature for the above unit -> unit
As you can see, we have an input after all! It's our good friend Unit. Great news, now we have an input and an output we can compose two functions easily.

```
let doNothing() = printf "Running doNothing" ; () // unit -> unit
let result = doNothing() // Run by itself
printf "Finished"
let chainedResult = () |> doNothing |> doNothing |> doNothing // unit -> unit
printf "Finished"
```

So now we know that all functions must have one or more inputs, and that they must have one output. Which means that every function in F# is composable, as long as output of one function maps to the input of the next function, they can be composed.

On top of this, with the above knowledge we now understand function signatures to their fullest (I think!).
If you want to know the output of a function, look at the last item in the list.
If you want to know the inputs, look at each item before the last item.
The more you see function signatures, especially of functions you write yourself, it becomes easier and easier to read them.

Nesting of function signatures in function signatures can become a little confusing, but that is where you should pay close attention to the brackets that denote the start and end of the function definition. It can remind me of some of the more confusing generic type signatures in C#.

Input unit ; Output unit
```unit -> unit```
Input string ; Output: unit
```string -> unit```
Input: Tuple of int & string ; Output: string
```int * string -> string```
Input: int, Function accepting int and returning int ; Output: int
```int -> (int -> int) -> int```
Input: int ; Output: Function accepting int and returning int
```int -> (int -> int)```
Generics- Input: two objects of type a' as separate arguments ; Output: List<a>
```a' -> a' -> List<a'>```

[Try today's code](https://try.fsharp.org/#?code=DYUwLgBAJg9gcjMALAlgOwOYAoCUEC8AsAFARkQAOATumAGYQBEASgK5probTyKqaMS5CLhIlQkKiADOrYJHw8EyLqOIlqtBowBi6FNKQgog9cQkQAxkgCG6Y8xlyFIvAB8AfEr5cIn7yqYfl6wyvwYYsSaaPRMepyGxoxAA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)