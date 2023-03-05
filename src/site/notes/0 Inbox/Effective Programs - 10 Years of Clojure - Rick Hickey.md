---
{"dg-publish":true,"permalink":"/0-inbox/effective-programs-10-years-of-clojure-rick-hickey/"}
---

tags:: #source/video [[4 Archive/Imported/Clojure\|Clojure]] [[3 Resources/Programming\|Programming]]
author:: [[Rick Hickey\|Rick Hickey]]
[Source](https://www.youtube.com/watch?v=2V1FtfBDsLU)

# Problems with Type Systems
"If it compiles, it probably works." — yeah, right. That's not true at all, at least not in the *difficult* way that programs need to work.

What if you created a class but now need to add one piece of information? New class? What's the name?

What if you're *missing* a piece of information? Everything becomes a Maybe?

Positional arguments are really difficult to manage.

The type of a thing really doesn't tell you what it is. String, float, etc. Millions of methods take the same types.

We define the meaning of a thing really only *in the aggregate* of a class. `name` makes sense as part of a Person. But the other guy added `name` to the `MailingList` class and now it suddenly doesn't work together.