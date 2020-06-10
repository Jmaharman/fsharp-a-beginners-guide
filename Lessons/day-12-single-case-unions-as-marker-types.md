# Day 12: Single Case Unions (SU) as Marker Types

Single case unions can help us to ensure the integrity of our data whenever we need it. Emails are an easy example of this because an email is represented as a string, but not every string is an email address. Let's say I need to write a function that will email a customer, one of the parameters I would accept is the 'To' email address.

In my email function I'll create a new instance of `System.Net.Mail.MailMessage`, which requires `MailAddress` as such:

```MailAddress from = new MailAddress("ben@contoso.com", "Ben Miller");```

`MailAddress` isn't particularly honest with us because if we gave it a badly formed email address, it will throw an exception at runtime. It would be nice if it gave us a heads up, without needing to delve into the documentation. If we knew at compile time that the possibility of an invalid email address is on the table, and that we should handle that fact.

I know what you're thinking, all email addresses _should_ be validated on the way into the system, so it shouldn't be a problem. In software lots of things _should_ happen, but things don't always work out the way we expect and we don't realise that until someone hits that path of the logic at a later date.

Yesterday we discussed how we can wrap primitive types with SUs to help avoid bugs at compile time. It turns out that an SU can help us here too. We can create a function that can act as the gateway to creating a valid email address.

This function can do this in a variety of ways, the first and simplest would be to return a None if the email address is invalid. That doesn't give us a particularly great user experience though, it would be better if we could provide an error message. To do this I'll use `Result<TOk, TError>`, it is an F# type which allows you to be more explicit than Some or None by providing a different result type on the Good path compared to the Bad path.

```fsharp
type EmailAddress = private EmailAddress of string

module EmailAddress =
    open System.Text.RegularExpressions

    let value (EmailAddress c) = c

    // string -> Result<EmailAddress, string>
    let create str =
        // Please do not use this, it's for display purposes only
        let emailRegex = new Regex(@"\b[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,6}\b", RegexOptions.IgnoreCase);
        let m = emailRegex.Match(str);
        if not m.Success then
            // If implementing fully, we could provide any number of failure messages
            Error (sprintf "\"%s\" is not a valid Email Address" str)
        else
            Ok (EmailAddress str)

 // We know, 100%, that this function receives a valid UN number, no extra checks required
let emailCustomer email =
    printfn "Emailed customer: \"%s\"" (EmailAddress.value email)

// We use an outer function to validate the input, and decide the error path
let contactForm emailInput =
    match EmailAddress.create emailInput with
    | Ok email -> emailCustomer email
    | Error err -> printf "%s" err

contactForm "john@johnharman"
contactForm "john@johnharman.co.uk"
```
[Try the code](https://try.fsharp.org/#?code=C4TwDgpgBAogtgQwJYBsCCATDAnCBnPKAXijGyQDcFhp5l0tcCoB7AMyj2HIDsBzALAAoYXBYYArilqJUmHPkJFhUVa0g8oAZRBcIcAHQAVCAA9gBgEoQ+UhNhimyipCx55hKtdOBQqKCWgACjo5RkUoAGMASmIozyE1KAB6ZM5uJH4oAFoAPihrPClgAB5QhgUCABp03j5cr1UfKNxqaC5sYkaklLSABWkEPGgMFigeFl8JYahgAAskPBqkYAByQjYWToxFsBQEEFIJbDAWYcI3FBBupOb9ems+MzieCAB3ApszIIABACIADoAIwA2mhsgAtAAM2QAnAYAPoAUgA1NkALoon5gyEw+EYlEAgw4iHogDeACYqgA2AC+wL+NUeZgA8mBgK53AYAJJ8Ca4ADCQwg0QA3DdvBBfHA4vdUMzTAYALLUSJzIIdMUS1RIDgTaUGLQSSKRCLzCA8bU9VJQbkcJBwPb6C0crJsKRXGpvaCRFhSDCkbAsChIDDQBA8Q48CRwIEQTrsKBsejHaBwRQIJ4eRI9XMwbBBzoasiZYAcQF-JF4AF-KCLcaTKAIPwIFCh2CyFBQeRMPC1zVWiAoYZWpIsgDWUBCnZ7EQHOdzSRUNoA6tBxxM3jUAIxQqFImrzaizBYbCQ8SIctxQXCmyj4JsttsBgCqADlxjG49gahMoGZuGbNUIEicdCFwABHCQkFwDBhDuTsBWmYAWHTTo5S7ZQF0DUs2E0P5yggANImQ1D4wALigGsqxrWtp3oWcCAMfxAn-TtogSVdoGmcNND9GhOndC8r00FCn1DNoT2gTIwAkYAagjAMw0iUNoHNf8Cy2UhqDmeCpSiNxgAQS8ADEthlDDuR4WTfCwpJEGANUOwY8ImMiVoaDY+grJsqA3hWXTsIAHygCcvNQHJ8gwpCuDI9DO26EL80LDTOjyHCeDLKBKz7VKEl9TLjOAMzsBlP4ACsWDmHgfkq6q5nsRAeD+YQCqM0zzOyuqau6hrSojAxfQMCRxxaoQgA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

I know this is not the best example in the world but hopefully you can consider the above concept and apply it to a project you are working on where a string is not simply a string. It's great to know that if you use an SU as a marker type, with the appropriate validation being done at the creation of the type, you can be sure it is safe to work with and hopefully it will be more obvious to anyone working on the codebase what that type is, more than simply being a string.

These SUs don't have to be strings, they could be any type. Imagine a `Customer` type, and the concept of a `PriorityCustomer`. Perhaps only certain features of your producte are available to `PriorityCustomers`. If you write the necessary business logic into the creation of that SU, then you can be sure that any developer trying to use your function knows that the customer must be a `PriorityCustomer`, and the decision on whether that customer is a priority customer or not is declared in one place.

Here are a few more examples of encoding business logic into our types.

A list that must have at least one item:

```fsharp
type AtLeastOne<'a> = private AtLeastOne of 'a list
module AtLeastOne =
    let create inputList =
        match inputList with
        | [] -> Error "One item must be provided"
        | _ -> Ok (AtLeastOne inputList)
printf "%A" (AtLeastOne.create [])
printf "%A" (AtLeastOne.create ["John"])
printf "%A" (AtLeastOne.create [1])
```
[Try the code](https://try.fsharp.org/#?code=C4TwDgpgBAgsAyECGBnYB5AdhAPAciQD4oBeKMAJwEsA3JYaORVDbKAewDMoCoAbKmgCwAKFEBbdgBMArn0YJkaLNBKioG-hGBQAxhWQMoVTGBkJBOtSM22o4+roAWx0+fiWoAdyrAn6uw0AHygAbQBdKABaYgBRCgp2CigAIhVjBnF7GTQoACNoSnYaKikIKRSAwJCAfWjidABrKAAKJiVWaBMzCzQASlFRShNgbhSAUhgU1vaWFQA6fUNoCIGRYcxR1MnptsU57EWDehWUgCl2J0wU8LWNrYmpmf3lQ6WTsIBGW9EgA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

A string that cannot be longer than 10 characters in length:

```fsharp
type String10 = private String10 of string
module String10 =
    let create (str : string) =
        match str with
        | str when str.Length > 10 -> Error "String must not be longer than 10 characters"
        | _ -> Ok (String10 str)
printf "%A" (String10.create "1234567890")
printf "%A" (String10.create "1234567890!")
```
[Try the code](https://try.fsharp.org/#?code=C4TwDgpgBAysBOBLAdgcwIwAYoF5YJQ2wHsAzKAZwLQFgAoegW2IBMBXAG2jiTS13pQhULsCgBjeBACGwaAAoq8KAC5K1VAEoBdYXqiNZ4gBbrlAd0TBjg-UIA+ZqOeMRkZgHQAZN6mtQAPih+AFoggFF4eGJlACIeQgM2KihkYjEAI2gOYjQIZWtpd34TaXhpcTl4CljbO0cAfSgwqAB5AGsoeQS+bCVNenowXmByWIBSAEFYrp6iD0kZOShY9AAmAGYAFgBWADYAdgAOAE5MWIG6YZRRlamZ7o0sBalZaFXN3cPTzABCC6AA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

SUs, along with DUs, Record Types and Option, allow you to represent any model with the business rules encoded into the types themselves. It can help us avoid bugs and it also helps point us down a path where we can see up front what we must account for. When you come back to your code in 6+ months you know exactly what you are expecting of that type, no matter how deep into the codebase you are. You won't need to refresh your mind as to what the domain rule around the string value of property X is. Is it always there? Does it matter if it's null or not?

You can do all of the above in C# of course, but the amount of code required to do it is verbose, hard to maintain and easy to have bugs creep in. That's not to say that you shouldn't! In fact I recommend being more explicit with your type modelling in C#, but be warned it can become tiring quickly due to the amount of code you need to write for it. In F# you would happily create these more constrained types for as many items as you wish, in C# I feel like I must pick my battles wisely and save energy for another day.

_I have had to re-write this lesson because I was using our internal terminology and context, I'm not entirely happy with how this version has turned out, and may come back to it in the future. Hopefully the contrived examples above give some input and food for thought._