# Day 22: Function type aliases

You thought I was done with type aliases didn't you? Didn't you! Well I'm not, because I didn't show you how to alias a function. Hopefully the idea of this is no surprise, functions are a first class citizen in F# after all. Let's bring back the CubeDimensions alias, and use it in a function signature:

```fsharp
type CubeDimensions = double * double * double
type CalculateVolume = CubeDimensions -> double
```

We've not seen a function signature for a while, hopefully can still remember the structure. If not here's a quick recap. The last item in the list is always the output, everything before that is an input with each item being separated by a ```->```. A function must have _at least one_ input, and *must have one* output.

Let's expand on the above and say that the CubeDimensions aren't provided to us semantically, but as a string. We'll need a function to parse a string and output a CubeDimension, we can define that as such:

```fsharp
type ParseCubeDimensions = string -> CubeDimensions
```

As we think about this signature, we'll probably start to think of scenarios that we need to consider and decide whether they are worth taking into account or not. In essence giving ourselves a guideline of what we are going to implement.

For example, as I've written the function signature above I realise I've not handled the failure path, what happens if the string isn't valid? Maybe I should make it an option

```fsharp
type ParseCubeDimensions = string -> CubeDimensions option
```

The problem with option is that my client would like feedback as to why the parsing failed. This means that my output naturally becomes a Result<Ok, Error>:

```fsharp
type ParseCubeDimensions = string -> Result<CubeDimensions, ParseError>
```

What should our ParseError look like? We can start hashing out some useful scenarios that would be good to feedback to the user pretty quickly.

```fsharp
type ParseErrors =
    | InputEmpty
    | UnexpectedFormat of UnexpectedFormat
    | MissingDimensions of MissingDimension list
    | UnitOfMeasureNotSupported of string
```

Before long you've created a pretty comprehensive set of types that describe the process from start to finish. All this with very few lines, and you've not even started implemented any code yet.

NB: I have used a variety of types below, feel free to jump back to any previous lessons to refresh your memory if necessary:

```fsharp
type Measurement = Measurement of double
type UnitOfMeasure = UnitOfMeasure of string
type CubeDimensions = {
    Height : Measurement
    Width : Measurement
    Depth : Measurement
    UnitOfMeasure : UnitOfMeasure
}
type MissingDimension =
    | Height
    | Width
    | Depth
type UnexpectedFormat = {
    Message : string
    Line : int
    Column : int
}
type ParseErrors =
    | InputEmpty
    | UnexpectedFormat of UnexpectedFormat
    | MissingDimensions of MissingDimension list
    | UnitOfMeasureNotSupported of string
type ParseCubeDimensions = string -> Result<CubeDimensions, ParseErrors>
```

Once you've got to this point, it's a great place to stop and review the design with another member of the team. Ideally the review may be as simple as sharing the code you have, depending on how much the types encapsulate the business logic and how much needs explaining.

As processes become more complex you'll find that writing out what you intend to implement, before implementing it, can give you better insight into what you are about to build, and how you should build it.