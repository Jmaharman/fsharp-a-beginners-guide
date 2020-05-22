# Day 1 - Tuples

First we'll start with what you already know from C#, and today that is a tuple.

A tuple acts in exactly the same way in both F# and C#, but F# provides a simpler syntax to it:

```let tuple = 1, 2```

You probably noticed the use of let, which you may recognise from other languages as the variable declaration. It is F#'s equivalent of `var`, but an important difference is that the variable is immutable by default.

Getting the values out of the tuple is a little different to C#, although in C# you can use the deconstruction syntax to do similar to:

```let one, two = tuple```

In fact as a rule of thumb I've noticed that commas are only ever used when a tuple is involved. I may be wrong there, but it does seem to be correct from what I've learned so far. You can see it above when creating the tuple, and you can see it in the second example when deconstructing the tuple into multiple variables.

If I only wanted one of the values out of the tuple, then I would use discard (underscore) to ignore the second value

```let onlyOne,_ = tuple```

Day one complete, congratulations. Feel free to play around with the above here:

[Try the code](https://try.fsharp.org/#?code=LAKANgpgLgBFCuAHSMC8MCMAaGAmUoiATgJYB2UAZjAEQAqAFhHEpAFwwCkAgjS8hALhoMAPZkIOKAHdRafpCHFyVWgGUIiAIZEtUZgDctYeBADOHTgBMYWsjet9xzGaKGRY4sAE8A8hKwAfXkEASVSCmoafx84JjEJGCMTCEsrJzIffwggA&html=DwCwLgtgNgfAsAKAAQqaApgQwCb2ag4CdMTJcMABwFp0BHAVwEsA3AXgCIBhAewDsw6AdQAqAT0roOSAMb9BAzoIAeYAPThoAbhkhMAJwDOJNgzAAzagA4OeQhqy5EhAEY9sYu6mBq3HvD6asEA&css=Q)
