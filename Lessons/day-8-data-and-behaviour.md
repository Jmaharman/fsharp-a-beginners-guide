# Day 8: Data and behaviour

In F# and functional programming in general, data and behaviour are kept separately. Bearing in mind it's built on top of the BCL it's not enforced, but it helps you to do this by default by using record types, discriminated unions and modules.

We'll cover each item in turn, but today's focus will be on record types. Record types are a super powered equivalent of C# POCO objects.

```
public class Address
{
    public string Line1 { get; set; }
    public string City { get; set; }
    public string PostCode { get; set; }
    public string Country { get; set; }
}
```

Here is an F# Record Type equivalent:

```
type Address = {
    Line1 : string
    City : string
    PostCode : string
    Country : string
}
```

Looking at the two there doesn't seem like much difference, but looks can be deceiving.

The C# POCO can be instantiated with as many or as few props as you want it to be, but with F# you must provide every single property of the record. You can of course do this with C#, but you will need to create the constructor yourself, or via resharper, and then make sure to keep it up to date. Not to mention avoid creating any bugs by applying the wrong argument to the wrong property, which I think we've all done in the past. F# generates the constructor for you, not that you interact with it in this way when creating records in F#. That's more for interoperability with the BCL.

For example, to create an F# record you do it as so.

```
let address = {
    Line1 = "221b Baker St"
    City = "London"
    PostCode = "NW1 6XE"
    Country = "UK"
}
```
[Try the code](https://try.fsharp.org/#?code=C4TwDgpgBAggJnAThAziqBeKBvAsAKCiKgBkBLAOwgEYoAuKFYRSgcwOKgGEzR7HmbDsQAKAeyZcxcaAyYsK7QsSkBXCsxD95Q-AF8CBADYRgUAIYJkaTDmFFyVWlgBEAJjfUARlABC5gGsIRCgAZWAXe25eLVcSMQo4BMjlInFJaWhXADkAdVoANgANAFEUzjUNRFioFwBVAGkUvSA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

When trying the example, if you put your mouse over the address variable you'll see that it is of type ```Address```. Again the compiler has done a bit of thinking for you, it knows that the only object available to you in the current scope with those properties is Address, it infers that's what you want and sets the type accordingly. If you edit the example to remove one of the properties you'll see it will warn you that you are missing a property, in this example you can remove all but one of the properties and it will still know what type you are trying to create, because no other type has a property of the same name.

The next feature you get with Record Types is immutability. The compiler won't let you change any value on an instantiated object. Where as C# is mutable by default. You can create immutability in your C# POCO so the properties are read-only, only allowing them to be set by the constructor. If any of your other POCOs are mutable, then you're out of luck, your object can be changed from under your nose. For example if you used a List, it can be modified by any other calling code. You would need to use an immutable version of a list. Remember that F# lists and the List as we know it are not the same. The List type from the BCL list is called a ResizeArray in F#, which I think makes it pretty clear what is happening.

Open the above example again and try to set a property on the address, you'll see the compiler won't let you. I've realised I've not been clear on the benefits of immutability before, if you think I should cover that, then let me know.

Penultimate feature, structural equality, meaning that when comparing record types it checks every value in the object before deciding if they are equal or not. Meaning two different instances that have the same values will be considered to be equal to one another.

```
let address = {
    Line1 = "221b Baker St"
    City = "London"
    PostCode = "NW1 6XE"
    Country = "UK"
}
let address2 = {
    Line1 = "221b Baker St"
    City = "London"
    PostCode = "NW1 6XE"
    Country = "UK"
}

let doAddressMatche = address = address2
printf "%b" doAddressMatche
```
[Try the code](https://try.fsharp.org/#?code=C4TwDgpgBAggJnAThAziqBeKBvAsAKCiKgBkBLAOwgEYoAuKFYRSgcwOKgGEzR7HmbDsQAKAeyZcxcaAyYsK7QsSkBXCsxD95Q-AF8CBADYRgUAIYJkaTDmFFyVWlgBEAJjfUARlABC5gGsIRCgAZWAXe25eLVcSMQo4BMjlInFJaWhXADkAdVoANgANAFEUzjUNRFioFwBVAGkUg3wTM0skVBQ3WzxU0koaW3dPH38gkPDylRjh+MTkqPTgKRlhvMLS6aJKzWHG5sNW0ygk+E60AFlzYABjAAssiysu2w7rboIwBWAAM1qAKReFynMTnD7XO6PIA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

Note that we are using ```=``` to compare the values. How can that be? Remember data is immutable by default so you can safely use = to compare two objects. Setting a mutable value, which is possible, uses a different syntax which means you can never accidentally mutate a value using the ```=``` operator.

Again you can do the above with C#, but now your code for this simple POCO has become a LOT more verbose due to all the hoops that you have to jump to with regards to HashCode etc, not to mention making sure you don't cause any bugs by missing properties or compare the wrong items. Hand rolled code for this is often going to have issues.

Let's finish off with a problem that immutability brings, how can I store a new version of an address without needing create the whole object again? We use the with syntax:

```
let nextDoor = {
    address with
        Line1 = "221a Baker Street"
    }
```
[Try the code](https://try.fsharp.org/#?code=C4TwDgpgBAggJnAThAziqBeKBvAsAKCiKgBkBLAOwgEYoAuKFYRSgcwOKgGEzR7HmbDsQAKAeyZcxcaAyYsK7QsSkBXCsxD95Q-AF8CBADYRgUAIYJkaTDmFFyVWlgBEAJjfUARlABC5gGsIRCgAZWAXe25eLVcSMQo4BMjlInFJaWhXADkAdVoANgANAFEUzjUNRFioFwBVAGkUg3xjUygqAA9gABExMRCsPFSLK1R0AHdeAAsozkcaW3dPcz9A4LDmCFNy4ha2syT4JHGAWXNgAGNprNGTmywu3v7EAjAFYAAzWoBSLxcoEcxmhzlcbkA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)

So there we have it, the Record Type. A very light syntax with a lot of benefits. There are more features available on a Record Type, such as being able to add methods to it. We can leave that for now though.

A good way to think about a Record Type is that it is a group of properties that must be declared something. They are an AND of properties, you have to have them all, as Scott Wlaschin so beautifully put it in his book.