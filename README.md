# fsharp-a-beginners-guide

This repository is a series of small lessons that I wrote for my colleagues at work, in an attempt to get them interested in F# with the possibility of using it at work in the coming months / years. My journey to F# was not quick or direct, in fact it's still very much ongoing, but I think these lessons have helped get the team interested, even if we haven't yet started using it on commissioned work yet.

I've accumulated my knowledge through various sources. Firstly by listening to Dave Mateer ([@dave_mateer](https://twitter.com/dave_mateer)) give a talk at [@dotnetsoutheast](https://twitter.com/dotnetsoutheast), where he walked us through his recent learnings from what he refers to as the "Orange Book", aka [Functional Programming in C Sharp](https://www.manning.com/books/functional-programming-in-c-sharp). He talked through his learnings from the book, which he'd also discussed in a recent [blog post](https://davemateer.com/2020/03/06/Orange-Book-Functional-Programming-in-C-Sharp). I thoroughly enjoyed the talk, but when looking into using one of the referenced libraries, [Language-Ext](https://github.com/louthy/language-ext), I found it all rather daunting. The documentation was definitely not written for someone learning a functional approach to programming, although [Paul Louth](https://twitter.com/paullouth) did provide a very interesting set of [wiki pages](https://github.com/louthy/language-ext/wiki) talking through various subjects. I read the Orange Book from front to back in a fairly short time frame, and found the concepts appealing, although the implementation was often frustrating for someone new to the subject. I found it hard to realise where the limitations of the approach were, and would often battle generics and extension methods to try and implement what I wanted.

A few months later I saw [Mark Seemann](https://twitter.com/ploeh) promote his Clean Coders series, [Humane Code](https://cleancoders.com/series/humane-code-real), on twitter. I decided to dive in and watch all of the videos in one evening. My main memory from the series was the concept that we read a lot more code than we write, and that optimising our reading of code would mean that the code we write can more expressive and make us more efficient. In the series Mark discusses various concepts, which I won't try to paraphrase or summarise because I would likely fail, but a lot of what was discussed was based on functional principles. Which to be honest felt hard to take on in the first watch through. After watching the series I then progressed with applying functional practices in C#, this time trying hard to use Language-Ext some more. Although it made more sense this time around.

This journey was both enjoyable and frustrating in equal measure. Everytime I solved a problem I found another problem that I didn't know how to solve, mostly because I wasn't sure how to compose the various generic typed extension methods to do what I wanted.

I watched more YouTube videos from Mark Seemann, and others on the subject of functional programming too. At the start of 2020 I realised I wanted to start learning functional programming properly, and started preparing myself for the fact I was about to dive into a world that was harder than anything I'd attempted before. I'd been hearing about Monads, Applicatives, Lambda Calculus and function signatures for a while now, and while I understood some of the concepts, I couldn't explain them.

Luckily I came across [Isaac Abraham](https://twitter.com/isaac_abraham)'s talk on YouTube called [Go Pro on .NET with F#](https://www.youtube.com/watch?v=UXeFR5RQnjs). It was a breath of fresh air, I found it easy to understand and there was no complicated terminology. With a renewed hope of being able to learn the language fairly quickly, I turned to the often suggested [fsharpforfunandprofit.com](https://fsharpforfunandprofit.com), written by Scott Wlaschin, but I found this a hard resource to learn from. In hindsight I can recommend it as a good reference for syntax and concepts if you need a refresher. After that I found Isaac's book, [Get Programming with F#](https://www.manning.com/books/get-programming-with-f-sharp). While the book is a few years old most aspects of the book are still relevant. The only area where it is now dated is with tooling, of which [Ionide](https://ionide.io/) is a great editor plugin for VS Code, while Rider cannot be rivalled for solutions that include C# and F# code. With my desire to know more I then dived into [Domain Modeling Made Functional](https://pragprog.com/book/swdddf/domain-modeling-made-functional) by Scott Wlaschin which cemented my intent to start writing F# at work.

It was at this point that I discussed F# to my colleagues, who were interested but also skeptical. I knew they wouldn't find the time to sit down and read either book, which meant that writing F# at work was a lot less likely. I could try and get them to watch Isaac's Go Pro on .NET with F# video, but that is unlikely to happen at an hour long in run time. It was at this point that I thought I could explain the core features of F# that make it so simple to use, but powerful at the same time. Not only that, but I'm still fairly new to F# and would likely relate to how the rest of my team think, rather than being an expert in F# and forgetting about some of the basic concepts that you forget about after some time. These explanations may not be 100% correct, but they would get my colleagues 80% of the way there.

One aspect that I've not yet covered in my lessons is the concept of using F# to serve HTML from the back end with the likes of Giraffe and Saturn. Not to mention writing front end code with Fable using Elmish and MVU. Each of these aspects has been encapsulated in the SAFE stack. A few great resources that I have found on this subject are:

* [SAFE stack](https://safe-stack.github.io/)
* [The Elmish Book](https://zaid-ajaj.github.io/the-elmish-book/#/) written by [Zaid Ajaj](https://twitter.com/zaid_ajaj)
* [Reinventing MVC pattern for web programming with F#](https://www.youtube.com/watch?v=deHj2lG5qOY) by [Krzysztof Cieślak](https://twitter.com/k_cieslak)

I'd like to give a big thanks to all of those I've mentioned above, without them I wouldn't have learned all that I have about F# so far. My hope with this repository is that it will help others learn F# in small increments, without needing to commit too much time. I think all the lessons can be read within 20 minutes. I'd recommend re-writing where necessary, based on the experience your team has and the understanding they have with the various concepts that we discuss. It's also important to encourage the team to play with the code as much as possible, this helps because the new syntax to C# and curly brace developers can feel alien to read at first, the more you write in F# the quicker it becomes easier to read and more natural to write.

Learning F# has been useful in many ways, not least because it has helped me to embrace what C# is and isn't. There were times during this functional journey where I was trying to push C# too far, I wasted time trying make something work where it really isn't appropriate, even if it is technically possible. I think my C# has improved in this last year, and that in itself makes it worth learning new concepts and approaches.
