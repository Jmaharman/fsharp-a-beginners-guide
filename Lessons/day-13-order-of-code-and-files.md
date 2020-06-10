# Day 13: Order of code and files
A light lesson for the day, but no less important.

You may not have realised so far, but the order that you write your code means everything. Generally speaking any code that you wish to reference should be declared above it in the order of the project. For example, if we rehash the `Address` record type from the other day, but declare the SUs below it, the compiler will error.

```fsharp
type Address = {
    Line1 : Line1
    Road : Road
    City : City
    PostCode : PostCode
    Country : Country
}
type Line1 = Line1 of string
type Road = Road of string
type City = City of string
type PostCode = PostCode of string
type Country = Country of string
```
[Try the code](https://try.fsharp.org/#?code=C4TwDgpgBAggJnAThAziqBeKBvAsAKCiKgBkBLAOwgEYoAuUymg4qAJQHsBDOe97uC2IBhMqD6jQQogAUOKYMI5xoDOQqUrpUJQFcKwRCAkd9hkAQC+BAqEiMqtLOUdQOAMygLElAOa3waE4eTH4Qjy9DPwD7SWMsOLdPb2j8O2h1RWVoLEzNaAiUin80wJ1TAyNQvUrjQqjioA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

As you'll see when you go to the above, it doesn't compile. If you really really wanted to have the other types defined below instead you can use the and keyword, but from what I've seen it's not that common and I don't think it helps readability.

```fsharp
type Address = {
    Line1 : Line1
    Road : Road
    City : City
    PostCode : PostCode
    Country : Country
}
and Line1 = Line1 of string
and Road = Road of string
and City = City of string
and PostCode = PostCode of string
and Country = Country of string
```

It's all well and good saying that in a single file the types / functions you reference must have been defined already, but how does that work when your files are ordered alphabetically on the file system? Well it turns out that they aren't. Files in an F# project are ordered as you define them. This may feel like a step backwards but it brings a lot of benefits.

If I gave you an C# project to start exploring, where would you head for? Program.cs probably, or perhaps Startup.cs. What if it is a library, where would you head to then? In F# there is only one place to go, and that's the last file of the project. In there will be the entry point or public API functions of your library.

That's a small benefit, the big benefit comes in how it forces you to consider the structure and dependencies of your application as you develop, without slipping into accidental cyclic dependencies, or quick wins by referencing the presentation infrastructure in your business logic.

Another aspect of project structure that will feel strange for a C# developer is the fact that in F# you will have many types, along with related modules, within one file. A juxtaposition to the C# world of one class per file. This is mostly because the syntax of F# is so light, that it would be crazy to split out every type into a new file. As you've we can create what would be quite complex models and functions in under 20 lines of code. It's worth pointing out that we aren't aiming to write as few lines of code as possible, but we are aiming to only write as many as we need to.

when managing F# code you'll often think about files, rather than folders.