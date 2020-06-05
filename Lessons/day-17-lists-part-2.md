# Day 17: Lists (part 2)

Yesterday we looked at how to prepend to a list, and why we prepend rather than append. Let's work through some more examples of common behaviour with lists.

Concatenating lists is simple enough, we can use the @ operator or the List.concat function.

```fsharp
let names = ["john";"peter";"paul"]
let moreNames = ["don";"phillip"]
let combinedLists = names @ moreNames
let combinedAlt = List.concat [ names ; moreNames ] 
printf "%A" combinedLists
printf "%A" combinedAlt
```
[Try the code](https://try.fsharp.org/#?code=DYUwLgBAdghgtiAzhAvBA2gIgFYHsAWUmA3JgA7ggBOJ5MArsJgLoCwAUKJHLlSAHLwkqDJgAmuIqTL4AlsGCyyLDlwgBjXHABGsqCDEAZWYjDI0sBMgACEHn0FXV4DVt36xAQWCQ0x0wB0mlDqMJDo0ELIxHa8AlEQzBAcZFR6YABmEJgApJ6Yrjp6Bv5mKWlQmdl5BZpFHt5gQA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

We've created, prepended and combined. We've sliced and diced our lists into smaller lists, but we've not discussed how to get an item out of the list.

The F# equivalent of First is called head:

```fsharp
let firstItem = List.head ["peter";"paul"]
printf "%s" firstItem
```

Just like C#, if you try to get First() out of an empty list, it will throw an exception.

If you are unsure as to whether there is an item in the list you can use tryHead, which gives you a Some / None:

```fsharp
let head = List.tryHead ["peter";"paul"]
let headNone = list<string>.Empty |> List.tryHead
printf "%A" head
printf "%b" headNone.IsNone
```
[Try the code](https://try.fsharp.org/#?code=DYUwLgBAFiCGAmEC8EAyBLAzmAdGATgJ4ASciA2gEQBWA9lAHaUDclA7usMJQLoCwAKFCQYCAHK0GIZBGBYwAHmz50DAOYA+HAFEAtgAcwhCAB8NaeXiKkEg-SoZgAZhEoBSAIKVoZOw+eubgBG3qLwElI4AJKYESBAA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

The thing about having an Some or None is that it is likely you're going to need to do something with it. If that is the case you could use an if statement, but more likely you would use pattern matching.

If we are doing the above, let's skip to the chase and go straight to pattern matching with the List pattern:

```fsharp
let takeRegister who =
    match who with
    | [] -> printf "Where is everyone?" // 1
    | [ "john" ] -> printf "john, thank goodness you are here" // 2
    | [ a ] -> printf "%s, where is everyone?" a // 3
    | [ _ ; "john" ] -> printf "I don't care who is first in the list, john is all that matters" // 4
    | [ a ; b ] -> printf "Thanks for turning up %s, %s" a b // 5
    | _ -> printf "Register taken: present %A" who // 6

takeRegister []
takeRegister ["john"]
takeRegister ["peter"]
takeRegister ["peter" ; "john"]
takeRegister ["peter" ; "paul"]
takeRegister ["peter" ; "paul" ; "don" ; "phillip" ; "john"]
```
[Try the code](https://try.fsharp.org/#?code=DYUwLgBGCGDWICUQHMCWBnMIBOEDuAFgPYQC8AsAFAQ0QC20YAxgfsfqmAVbRAD4QA2gF0IAWgB8EAA7ZUAOzAAzCACIA6gRwgIGCCABuOAJ5F5IAPyqIAehsQAjD1oDBagFZEC866Mky5RRVVT28AGigCaHlYCGQiIgATc3R0CFMAVwhobB0tXOs7CAAmZxpXbIg-KVkFZTUAUnQIwm1dNMMTM0traFt7AGYy-iEIAH0IAG4PLx8q8RrA+tUASQhEswBySCYcnUISPSVUbExdeUidYAwwCNCLvWhgYEjGekYsU8L7ABZhir60wARvN-LUgmoACpRGJpJREXBgDLYeQKZAQDLSCBNCJNXoQEFFACs-3GCwCdWCSDQmBwUDgIHkAC4AiB0IzIA0AILWA79CAANioVBg8GpNzpIhFDPFtNwghCs1UwmlYpQEvlqmk4BwytViHVcqEWp12Gs00V3j1lFFBppn2N2s+5rU0mgGWA1ttsodCqduqmrvdnsDqg2cwt0gIqGeqGkLstPmEQA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

I tend to think of the list pattern as replicating the list structure that you wish to match. I'll explain what each pattern represents:

1. An empty list
1. List of one item combined with constant pattern, we specify what that single item should match "john".
1. List of one item, where the value is placed into the ```a``` variable
1. List of two items, we discard the first value because we don't care what is in there, but the second item must match "john"
1. List of two items, where the first value is placed into ```a```, and the second into ```b```
1. The classic discard option, where by we are accepting anything not yet handled

As you can imagine the number of combinations is endless.

There is another common practice with lists, which is where you match on the head and the tail. We've already seen the use of head above, this is the first item in the list. So what is tail? Well if you think back to our Cons, it's a tuple of T * List<T>, therefor the tail is the remaining list. That tail may be empty, but it could be another Cons, which will once again have a T * List<T>. Using the head and the tail, you can recurse through a list item by item.

Before we jump into the below, I should quickly explain that in F#, if you would like to recursively call a function you must explicitly state this to be the case. To do this you add the ```rec``` keyword after ```let``` but before the function name. I'll tag the order that our code executes, and explain each point in turn.

```fsharp
let takeRegister students =
    let rec callRegister students =
        match students with
        | [] -> printf "End of register" // 8
        | head :: tail -> // 5
            printf "%s? Here" head // 6
            callRegister tail // 7

    match students with
    | [] -> printf "No-one present" // 1
    | _ -> // 2
        printf "Start register" // 3 
        callRegister students // 4

takeRegister []
takeRegister ["peter" ; "paul"]
```
[Try the code](https://try.fsharp.org/#?code=DYUwLgBGCGDWICUQHMCWBnMIBOFMFcATEAOzHQgF4BYAKAgYlEmxAGMI3phgk1MceMEVLkqdRpIgBbaGDYALISLIUA7qjAKJUhgB8IAbQC6EALQA+CAAdsqMgDMIAIgCiJQhAD2T1vyzYzhAA9MEQABw6ugYKINCeAFwJUNCowOZWoRAArFG6jLb2YE7OAKToAPwQABI4IEGx8SFhAGx5+ZzcvCgYASlpzRAA7HR5svJKBMSqEBpaeQYmGTZ2ji4Acl5mXiQgKyDookFZAIwLEAD6y1kATO0Fq8UuAMow2Cw9AoGDAMwQ9wwuDw+L1BFNRBQsgAWUa0GDwEFfIzGOiFNbOMyYrGY5x0eGIT59QzOazgHBBADcLms0HwwGcxiAA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

I've updated our takeRegister function to now print out the register as it is taken.

1. There are no users in the register, this is the end of execution
1. At least one student available
1. Print that we are starting registration
1. Calling the register with our list of students, this starts the recursion
1. Pattern match on the head & tail
1. Print the studen's name
1. Call the recursion again with the rest of the list, the tail
1. Eventually the list will be empty and end up here, at which point we print the end of the register

There we have it, the basic concepts of lists covered. Everything else is available on the List module, if you take a look on try.fsharp.org you'll see quite a list!

As I've mentioned before lists are the go to in fsharp, but that doesn't mean you can't use the same concepts for an array when pattern matching. The difference is the syntax of an array, which looks like so:

```fsharp
let students = [| "peter" ; "paul" ; "don" ; "phillip"; "john" |]
printf "%A" students
```

F# also has seq, which is an equivalent to C#'s IEnumerable. I won't go into this now but it's worth knowing that it exists and it's purpose. As long as you know where using IEnumerable in C# makes sense, you'll probably get the gist of seq in F# too.

Each type of collection has it's own set of methods, you can access these the same as you would access any function for a type, by looking on the module itself.

Learning how lists work is pretty important in realising how to interact with them and now we know the implementation we can understand why we interact with it in the way that we do, as well as what approaches are available to us.