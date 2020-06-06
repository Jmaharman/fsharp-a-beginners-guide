# Day 4: Pipe Operator

Before I start on what it is, let's discuss a problem it solves.

We have the following calculation, `2 + 3 * 5`, and we'd like to represent that in code. Let's define a few C# functions to help do that.

```csharp
int Add(int x, int y) => x + y
int Multiply(int m, int z) => m * z
```

As we all know from school mathematics, we have to apply the multiplication first, and addition last.

```csharp
var result = Add(2, Multiply(3, 5)); // Output: 17
```

While it is correct, it's not as easy to read, because as you scan left to right you are seeing that you need to Add something, but then you can't add that until you have got the output of the multiplication.
In our heads we'd do the multiplication first, but we can't do that first unless we got the result first and passed that into `Add`. Like so:

```csharp
var multiplyResult = Multiply(3, 5);
var result = Add(2, multiplyResult); // Output: 17
```

Not too bad right now, but if we added more inputs to our calculation, the more verbose our code becomes. How can we solve that in C#? Extension methods!

```csharp
static int Add(this int x, int y) => x + y
static int Multiply(this int z, int m) => z * m
```

Now we can write our code as such:

```csharp
var result = 3.Multiply(5).Add(2); // Output: 17
```

That feels better to read, we're now able to chain the result from the first function into the second function. It certainly reads better.

So how does F# deal with this problem? You guessed it, the pipe operator ```|>```

The pipe operator is an operator that receives an input value and a function, finally returning the value of the function provided, the signature of it looks like this:

```fsharp
a' -> (a' -> b') -> b'
```

I know the above looks daunting, but the apostrophe denotes a generic type, which in F# normally start from a and work up through the alphabet. You may have also spotted the brackets, those denote a function.

In C# terms it would look like this:

```csharp
Func<T1, Func<T1, R1>, R1>
```

If you don't quite get the above, don't worry, signatures can take a bit of time to get used to reading, we'll cover those more over time.

The short of it is that the pipe operator only takes in one input, a mapping function and outputs one value.
Now for some F#. Let's create two functions to do the mathematics

```fsharp
let add x y = x + y
let multiply m z = m * z
```

As mentioned above though, the pipe operator receives one input and gives one output. How can we change our functions so they only take one input and one output?
Currying, using our Curried Function

```fsharp
let add2 = add 2
let multiplyBy5 = multiply 5
```

Which means we can now use the pipes:

```fsharp
let result = 3 |> multiplyBy5 |> add2 // Output: 17
```

Great, but we've sort of run into the same problem we had before in C#, because we were having to declare some variables before then pulling everything together for the final result.

Luckily for us we don't need to declare those curried functions before the pipe operator, we can define them inline and it will work exactly the same

```fsharp
let add x y = x + y
let multiply m z = m * z
let result = 3 |> multiply 5 |> add 2 // Output: 17
printf "%d" result
```

And that is the Pipe Operator, it helps you to compose your functions into chains of functions, just like Extension methods, but without the need to explicitly create a static function using the "this" keyword. There are other benefits too, but I'll leave those for now.

[Try today's code](https://try.fsharp.org/#?code=DYUwLgBAhgJjEA8IE8IF5EQNQoLAChRIBbAV2DAEsAHYVYiAL3QgYComCCiIAnEAM7lIGAMwQAPgD5WwmnQgBWSTNjwATBAD0WiAHlSYaoYBcEAIwB2Lvmq9KAOzAAzCACIApDDd9BwoA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)