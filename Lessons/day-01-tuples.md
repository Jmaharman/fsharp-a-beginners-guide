# Day 1 - Tuples

First we'll start with what you already know from C#, and today that is a tuple.

A tuple acts in exactly the same way in both F# and C#, but F# provides a simpler syntax for creating them:

```fsharp
let tuple = 1, 2
```

You probably noticed the use of let, which you may recognise from other languages as a variable declaration. It is F#'s equivalent of `var`, but an important difference is that the variable is immutable by default.

Getting values out of a tuple is a little different to C#. However, to those familiar with C# tuple de-structuring, F# follows a similar pattern:

```fsharp
let one, two = tuple

let three, four = (3, 4)
```

In fact as a rule of thumb I've noticed that commas are only ever used when a tuple is involved. I may be wrong there, but it does seem to be correct from what I've learned so far. You can see it above when creating the tuple, and you can see it in the second example when deconstructing the tuple into multiple variables.

If I only wanted one of the values out of the tuple, then I could use an underscore to ignore the second value.

```fsharp
let onlyOne, _ = tuple
```

It should be noted that the F# standard library has the functions `fst` and `snd` defined so the following will achieve the same result:

```fsharp
let onlyOne = fst tuple
let onlyTwo = snd tuple
```

Day one complete, congratulations. Feel free to play around with the above here:

[Try the code](https://try.fsharp.org/#?code=LAKANgpgLgBFCuAHSMC8MCMAaGAmUoiATgJYB2UAZjAEQAqAFhHEpAFwwCkAgjS8hALhoMAPZkIOKAHdRafpFCRYUBkQiSYlUfCLyAFAGYcAFgCUQ4uSq0AyhEQBDIo6jMAbo7DwIAZw6cACYwjmTBQXzizDKiQspiZGAAngDyElgA+vIIApakFNQ0aclwTAkeXj4BgZGJqRJxIuLJdLLyvmEKgiCE+TZFdaXRbZ7eENW1LbJAA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)
