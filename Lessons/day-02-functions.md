# Day 2 - Functions

Today's lesson we will start on functions. Functions are a first class citizen in F#, which they are meant to be in C# too, but in a lesser way. In C# there are Method Groups which are the functions you define on your classes, static or otherwise. You also have Func<T> and Action<T>, which is what you pass into methods like Linq Select, Where, statements. A Method Group is different to a Func or Action, although it can be cast to one when necessary. You all know how to make a method group in C#

```csharp
public class Example
{
    private int Sum(int x, int y)
    {
        return x + y;
    }

    public void UsingDifferentFunctionsTypes(int inputX, int inputY)
    {
        // You have to define the inputs and outputs first when declaring as a func variable
        Func<int, int, int> variableSum = (x, y) {
            return x + y;
        }
        // Local functions allow you to define them as you would normally, except only available the current scope
        int LocalSum(int x, int y) {
            return x + y;
        }
        You can call each of them in the same way
        Console.WriteLine(UpperCaseMethodGroup(inputX, inputY))
        Console.WriteLine(InnerFunction(inputX, inputY))
        Console.WriteLine(variableFunc(inputX, inputY))
    }
}
```

In F# a function is very much a first class citizen, of which there are two types, but we'll start with the one you'd probably find easier to read, although it is used less, for reasons I'll explain tomorrow as this has taken too long going over C# stuff :wink:

```fsharp
// Function definition
let sum(x, y) = x + y
let result = sum(1, 2)
printf "%d" result
```

[Try the code](https://try.fsharp.org/#?code=LAKA9GAEBiCuB2BjALgSwPb0gEwKYDNV5U1NQAbXZSAZ1gFsAKADwBpIBPASkgF5JmkANSdQEGAhQYssGgEMA5rgpVIAJ1x1y1fnSYBGdgCYuoUAAc1RZPkgAiAKTY76zbG1A&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

You may notice that there is no need to define the types of those arguments, that is because the F# compiler can extrapolate the types you want based on the fact you are using the + operator on them. That may be scary at first, but don't worry, when looking at it in an IDE it will tell you what the signature is, often shown above in something similar to this:

```fsharp
int * int -> int
let sum(x,y) = x + y
```

I won't explain the function signature just yet, but know that you can decipher what is expected in your function by reading that one line.
