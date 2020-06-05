# Day 16: Lists (part 1)

Today I want to talk to you about lists, specifically the F# list implementation. This is not your BCL's list implementation, of course that couldn't be the case because the List type from the BCL is mutable.

F# has implemented it's own version of a list using a linked list structure and most functional languages would use a similar approach. It's worth learning how the linked list is implemented so that when we interact with a list you understand why it does it in that way.

A linked list is a recursive type, you can think of it similar to the following type:

```fsharp
type List<'T> =
    | Empty
    | Cons of 'T * List<'T>
```

In the above I've implemented it as a discriminated union, where by it is either Emtpy, or it is Cons. Cons holds an item of T and a list of T, it does this as a tuple. You'll hear the term Cons frequently, so it's worth remembering that term and it's relation to a list.

Let's create a small list of items using the above implementation:

```fsharp
let emptyList = Empty
let paul = Cons ("paul", Empty)
let peterAndPaul = Cons ("peter", paul)
```
[Try the code](https://try.fsharp.org/#?code=C4TwDgpgBAMglgZ2AHgOQBUB8UC8BYAKCmKgB8oBRAWzFEJLKgGEB7AOwShYDMoMoAVLEQoMmQoQA2EYFAg1Q8JLkoKQUmVDABDAK6SVrDlAAUAIh36zAGlW0QASg2zIwCACcAgmwAmABT0DHGZ2TnNXDxstQKcCQjB3ODZgXjMAUk8zOTUlYHjE5NSMrMtJfKSUqHTMrRkPb39AoA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

If you look carefully above, you'll notice that we don't append an item to the list, it is prepended. Why would we do that? Immutability is the answer. We can't modify the last Cons to add our new item, because it is immutable. What we do instead is wrap the existing list inside a new Cons, this gives us a new list, with the additional item prepended.

The beauty of the above is that we haven't modified anything, it's a very efficient way of expanding a data structure. We continue to use the original list underneath, and thus only add a small amount of memory.

If you want to add an item into the middle of the list, fsharp be able to re-use some of the list, but the other half would need to be created from scratch:

```fsharp
let emptyList = Empty
let paul = Cons ("paul", Empty)
let peterAndPaul = Cons ("peter", paul)
let johnBetweenPeterAndPaul = Cons ("peter", Cons ("john", paul))
```

Above you'll see I have to create a new Cons for peter, as well as john, but I can re-use paul. This is why you would avoid inserting items into the middle of a list in F#, because it creates more allocations than prepending. It's not that you can't, but there is probably a better tool for the job, such as the BCL implentation, which is called ResizeArray in F#.

Creating a list in F# is very simple, you will have seen it before now but not necessarily realised it is a list. Let's create a list and prepended an item to it using the Cons (```::```) operator (see, I told you we'd mention Cons)

```fsharp
let emptyList = []
let paul = ["paul"]
let peterAndPaul = "peter" :: paul
let altPeterandPaul = List.Cons ("peter", paul) // equivalent to the above
```
[Try the code](https://try.fsharp.org/#?code=LAKANgpgLgBBC2AHKBPAMgSwM6wLwwG0BdUSWRAQwFcwZ8CAiSmhk8aGRaCAJwEEAdgBMACtVr4m3HgxgAuOZ3GkOFMFBHSKwsTToxMOAHQBhAPYCsMABRSovBgBolNAJQwA9B7gBHKhgA3NQgBWCgzGCgACwgYCgAjMwCIUFBEHgxQgDMYBgBSPllmMDSM7NyCoulBUWUQdMyoHPzCuPVNex5tWpogA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

The cons operator is built fsharp core and gives us a nice clean syntax. We use it in other scenarios too, but we'll get to that tomorrow.

So how do we interact with this list, other than prepending to it? Well basic list operations such as Map (Select), Filter (Where), Fold & Reduce (Aggregate) are all available on the List module.

They act in the same way as the C# equivalent extension method, give it the function you wish to map / filter / fold / reduce with, and then the list itself. With pipe it isn't drastically different either:

```fsharp
let newResult =
    names
    |> List.filter (fun str -> str.EndsWith("l"))
    |> List.map (fun str -> str.ToUpper())
```
[Try the code](https://try.fsharp.org/#?code=LAKANgpgLgBAdgQwLYQM4wLwwNowEQAO0EATnjANz4EICuY5AuqKJLHBAO4BKa9sGUDGHxkaISIA+APhgAZAJaooAOgBmCsFFIwAFGtpwYykjAC0skyoCicACaoA6gqgALXXgYBKLxOEz5JVUkBAI9AyMTc0soEhUAFQB7AFUCIhJdHxYQAhIFOCg1fABSAEFyDh4+LSA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

If you played around with the code above you may wonder how to get a specific item out of the list. For example, how do you get the 3rd item? You can use slicing:

```
let names = ["john";"peter";"paul";"don";"phillip"]
printf "%A" names.[0] // Index 0
printf "%A" names.[1..2] // Index 1 through to 2 
printf "%A" names.[..4] // Everything up until 4
printf "%A" names.[4..] // Everything after 4
printf "%A" names.[5..100] // No exception thrown if items not found
```
[Try the code](https://try.fsharp.org/#?code=LAKANgpgLgBAdgQwLYQM4wLwwNoCIBWA9gBZy4DcuADtBAE4XUICuYjAJoWZVcQJZgwfKrgC6oKnT5woAMxi4ApAEFc8ZGgB02AAyiYAegMwAknHYQAHjB0SpM+UtXqUqbQEZNmgEz6jp8ysYdxgoYjpCZgBzYlDCGG8YO2k5BRU1RFdtLwAWP2MAUQA3egBPMOkomGYqapkBGBzkhzTnTK1sHK98mGKyirgqhFkoekbm1KcMjTdsAFYvdx09Q2MAOXirAGMIKig+LlDwwgB3OBg+eT5RpHQ4QlhZSPMgA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

That's enough for today, but we'll cover some more list functionality tomorrow.