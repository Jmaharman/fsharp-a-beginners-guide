# Day 10: Modules

The last two days we covered the data aspect of F#, of which I have not covered exhaustively. You can still fallback on all the usual BCL features if you wish, such as interfaces, classes, abstract classes etc. In the middle of your F# code you'll gnerally stick to F# features, but when it comes to interop with other libraries, often those written in C#, you'll have to fallback on those features. I'd recommend getting your head into a book to get more indepth information on this subject on the whole, but I think this serves as a helpful first pass.

So what about the functions, or the behaviour of the application, where do we place these to keep them organised? I always imagined that F# being a functional language meant that you can just define a function wherever you choose, akin to JavaScript / TypeScript, but that's not the case. To write a function in F# you will _generally_ write them inside a Module. "Hold on John, we've been writing functions without any modules in sight for two weeks!", yes you are right. With the try.fsharp.org website we've been writing what are known as script files.

F# has three types of files, but the ones of importance for you at the moment are Source files (.fs) and Script files (.fsx). The last type is a Signature, but I don't know what that does yet.

Script files are essentially a scratch pad for you to run F# code ad-hoc, much like we have been the last two weeks. Script files are not compiled down into anything re-usable or executable, they are purely for running code ad-hoc. They are really useful for having a little play around with some code, much like you might test some C# in LinqPad. Any function written inside a script file does not need to be associated to a module. You can reference assemblies and execute code without the need to compile too, but let's leave that for now. In fact in F# 5.0 coming later this year, you'll be able to reference Nuget packages directly from your .fsx file, which brings some fantastic opportunities for scripting.

Source files are what you include in your project, they compile down into a dll just like your usual C# projects. However, to do that your function must belong to something, and in the case of F# they belong to modules. What are modules? You can think of them as static classes with static methods, which are public by default, but can be private too.

Personally I find there is often some ambiguity in the current ecosystem of C# as to where we should put functions. For example, these days we create a lot of POCOs and DTOs in C#, but where do we place the logic relating to these? Sometimes it's on the model, sometimes it's an extension method and sometimes it's in a handler.

In F# there are three common ways to layout your types and modules, but I think my preferred approach so far has been to define the Types up front, and then lower down in the same file define the Module for these types.

Below is an example of defining some types, and then some modules. We'll then use the functions we've defined to filter a list of customers based on their contact preference. I would recommend reading the below example at try.fsharp.org because it gives you a tiny bit of syntax highlighting, not much but it's something to help break down this large block of text.

```fsharp
// Define our types up front, and because declaring these types is so light in syntax
// you would declare many in one file, normally related to one another of course
type Address = {
    Line1 : string
    City : string
    PostCode : string
    Country : string
}

type PreferredContactMethod =
    | Email of string
    | Mobile of string
    | Letter of Address

type Customer = {
    FirstName : string
    LastName : string
    ContactMethod : PreferredContactMethod
}

// Now we'll define some modules on them, I've split them up into modules based on the Types name
module PreferredContactMethod =
    let isEmail contactMethod = // PreferredContactMethod -> bool
        // This is pattern matching, well see look at this soon
        match contactMethod with
        | Email email -> true
        | _ -> false

module Customer =
    let findAllEmailContacts contacts = // List<Customer> -> List<Customer>
        let customerPrefersEmail customer = // Customer -> bool
            customer.ContactMethod |> PreferredContactMethod.isEmail
        contacts |> List.filter customerPrefersEmail

// Let's make some customers
let customers = [
    { FirstName = "John" ; LastName = "Harman" ; ContactMethod = Email "john@johnharman.co.uk" }
    { FirstName = "Bill" ; LastName = "Harman" ; ContactMethod = Mobile "0800-dial-a-dev" }
]

// Print out the results to see that John "Never checks his emails" Harman is the only value output
printf "%A" (Customer.findAllEmailContacts customers)
```
[Try the code](https://try.fsharp.org/#?code=PTAEBEFMDMEsDtKgPYFcBOoAuBPADpAM6ip6jTrLxYA0oAhvACagBGkAxvaoUk5wBt66BAHNsACyJJcBYrGKFkoAbFESsoBKEI5q9AB4BYAFAhQONKADuaAS34ch6JAFtGOLfBSJysAZB08Mjo7gICni5CWJAsWMpUSIzIWFKYyNCgHGjovKaySACCTEwuhMQAvKAA3qag9aAAMgiQAIygAFw6WCLwonUNAMKwuJ3dvf0mDaAACsiEWIPI-GMLEwP1S6jU6J5da2KmAL6m+fhIMy7QkOguTEv6HFgAspCpy6AVG6AAPqAAou5-ChMgc+t8-s9kKx-EgMuNDlMGn9Gm8YulMsVSkRCKcTAVQIMePFXDdPjVvgAxWC5LAAOXopNWPUR00a9AWDKZ+xZ4KRmyoWHoT1e7xYXUuMBudweQpFbwky2OePMdOQ1hskAA5OFQPw4L4lEzXMtUAFiFRJJBXHQAJJagBuSEIeFUmlS1pIZAQ8VAJqYZqIbA5sR8VtAABVzsR4IzIKZ-YHZldpbFZcKXgqPl9+So3lpCID6MDso9M2LyeZJddbmnBRnRYqWABaAB8bGQyAE32m5gjEgUBdAeHoWHR3ncWA4A76dGskF1vCQAk7AGsGO6B4pO-Aew1J9OsvX5RXrCMJHv6n8i8DrcWBKA29h0Kh47npn8APqP9vQegCPITATU0AkJYlkFJTAc2mAJNANJhCnCG8BHTJ5iFLOUsEqUBzGaBYAB4iQWCCbnbJ88KwQjwMg1tLzzTQOGom5qxuQsgQfRjiMgyswCIkkySfVhO27d9pgaTj+PQAA6VDyybX52xY2t7mPOTlikhRkNAOiMIzYgfnbCipLgAR0SyJj0CUtj7xVMBUSwLViHcVdnRI8yuNY0xYPcyTsIAbW+apQGpWkuSQKoACIAClkAkeAItAABuJoOXpONyQigAJYR3HipLCVUxtswBdjQAigArWL4AAAUquKJByxgpOyKTUFXBKTlzIKQs5dLIoAIX8AQEuS9leqZSLstCRgRoKssipYKooRhUCIoABgADjWtbmyYWB-2behdsgB0OtMABdWzkx9FBUE3JAyjNLDsGUJdJFHUAYrisq6ROslp04VdiC3UA738QgEqm3Khw9HwIlAB1-1fW6sDwO7TDwXosEyCKAFJCgSgAKPiSOk+DEIEZDZPQizCAASiAA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

There is not as much to unpack in this snippet as it may look. The types you've seen before, they aren't new. In the modules we have created a few useful functions which I've added signatures to via comments for you to see, hopefully these signatures will help you unpack what they are doing.

Remember the last entry in the list is the output, and everything before that is the inputs. They are all separated by ```->```.

First the PreferredContactMethod module gives us a function to simply help decide whether the preferred contact method is email or not. It's using pattern matching, we'll come back to that beautiful feature another day. You can probably understand what is happening if you know the new C# pattern matching expressions.

Customer.findAllEmailContacts is a little bit more involved. First we define a new function to act as the predicate for the filter. The last line is the return value of the function, an equivalent in C# would be ```contacts.Where(customerPrefersEmail)```

Nothing too scary once you break it down. It is tempting to write the equivalent C# code in full, and give you an idea of the amount of code involved between the two, but I really can't be bothered to type out that much code for the sake of a size comparison.

You may find reading this amount of code harder to do outside of an IDE. When you are in a proper IDE it does make reading the syntax easier of course. In fact I would highly recommend it if you have the time.