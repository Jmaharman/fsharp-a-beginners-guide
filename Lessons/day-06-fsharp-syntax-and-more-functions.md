# Day 6: F# Syntax and More Functions

In Day 5 I explicitly avoided doing any multi-lined functions because I wanted to avoid the discussion around F# syntax. Specifically F# syntax for methods and scopes.

As you've probably noticed by now, we aren't wrapping every function in curly braces, nor are we adding semi-colons at the end of every line.

F# uses whitespace indentation to know what lines of code belong to that function. If the function is on one line, that's simple enough and requires no explanation, as we've seen before now.

Multi-line functions, or even variable assignments must start the definition on the first line, but the content of that assignment / function must start indented on the next line, with all other lines of that function following after with the same indentation.
For example:

```fsharp
let add x y = x + y
let multiply m z = m * z
let oneLineResult = 3 |> multiply 5 |> add 2 // All on one line like before, no problem
let multiLineResult =
    3                  // Multiple line expression requires you to start on the next line
    |> multiply 5      // The number of spaces you use is not important
    |> add 2           // But the indentation must remain consistent
```
[Try code](https://try.fsharp.org/#?code=DYUwLgBAhgJjEA8IE8IF5EQNQoLAChRIBbAV2DAEsAHYVYiAL3QgYComCCiIB7AOxAAZSoIBKIAM7lIGAMwQAPgD5WMmnQgBWJatjwATBAD0xiAEFgwPvxsgIwUSAA0ESbwj8P1AE68ARqDE3OBqFJQi4lIy6AQQ8RAKphAAsuq09o6CECAIvlKSlAIQPiAAjqSUpZIovKQQYB6SYFA+kMVgABb2ggiQWSBxCSphVLSoOskAKt2epMT+ID58AGZu1FAAxlK19aSS9pQ1XpCUxNS8bVD8YEPxI-oQRskAQqSQXSD0+5ClxFCiCCbASFZogG4EIA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

As I said, functions and assignments have the same syntactic rules, so if I wrote my doNothing function from Day 5 as multi-line, it would look like so:

```fsharp
// Originally
let doNothingOneLine() = printf "Running doNothing" ; ()
// New version
let doNothingMultiLine() =
    printf "Running doNothing"
    ()
```
[Try code](https://try.fsharp.org/#?code=PTAEHkCcEsHNoHYEMA2KCeBYAUCgpgC6gAmA9gHKkEAWis4CeAMongBQCUoAvKAA4wEBAGagARACUArggR0SFKrQSwxoANyhOOHCFDk8Ad1AA3PJADO0Ughz4iZSjToBZKSgLQWjTjxygA-kERcWlZeUclOjF-QM4gA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

You'll probably notice in both functions above I've not used the keyword return at all. That's not to say that the return keyword doesn't exist in the F# language at all, because it does, BUT for expressions it is not required. The reason for that is because we are in an expression and you cannot exit an expression early. The same as true for C#.

In C#, you can't do anything except return a value, however in F# you can define values in the function scope and use them later.

Remember that functions are first class citizens, which means you can define functions too. You may wonder how it it differentiates between one functions implementation, and the parent function? Indentation.

```fsharp
let toUpper value =
    let upperCaseChar c =
        System.Char.ToUpper(c)
    let someArbritraryValue = "not needed, but demonstrating"
    String.map upperCaseChar value
printf "Stop shouting %s" (toUpper "will ward")
```
[Try Code](https://try.fsharp.org/#?code=DYUwLgBGD2CqAO8QCcIDcCGwCuIIF4BYAKAjIlEm0RQGEMBnEWgCw1QGMCTzeIBlAJ4MwIALYA6VuwkAVODWQAKDgEoe5ShAbQxIAILIARsgCWYZO0EA1LLgIQARADtokZyBAATbwBoIRtiQPmLQziKWYKbOAOaOGmT8FtExEmIY8BDUSMj0TNKomDggJCTwZs5gAGZOSdCZDCzQQSkQAKQMjhBKMAg5TgDupsDAEAPsXo6qQA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

Because we're always expecting an ouput from a function, if we aren't using that value then there's probably something we've done wrong.

F# will warn you when this happens. Try and modify the above code like so:

```fsharp
let toUpper value =
    let upperCaseChar c =
        System.Char.ToUpper(c)
    let someArbritraryValue = "not needed, but demonstrating"
    String.map upperCaseChar value // You'll see a warning on this line
    String.map upperCaseChar value
printf "Stop shouting %s" (toUpper "will ward")
```

You can solve this two ways. The first would be to use let and assign it to a variable, which is probably what you want most of the time. Sometimes you don't need the functions output, and in those cases you can use the ignore function, to discard the value and remove the compiler error. Modify the appropriate line as such:

```fsharp
String.map upperCaseChar value |> ignore
```

Learning all of the above was a bit of a game changer for me. Not because it does anything spectacular, but because it was unspectacular. I'd tried to implement a more functional approach to programming in C# using the Language.Ext library and Query Expressions but it wasn't easy. So my imagination led me to believe that functional programming on the whole was hard, but really it's mostly different. Once you learn those differences there's a whole new world that opens up. One with a lot less boiler plate, and with a compiler that tries to do the heavy lifting for you.
