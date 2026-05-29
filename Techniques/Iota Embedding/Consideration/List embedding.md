---
tags:
  - Consideration
  - IotaEmbedding
author: Matt6049
---
Let's get this out of the way first. List [[Iota Embedding|embedding]] instead of [Introspection](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us/?nospoiler#patterns/patterns_as_iotas@hexcasting:open_paren)+[Retrospection](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us/?nospoiler#patterns/patterns_as_iotas@hexcasting:close_paren) is not necessarily evil. It doesn't hurt you. Yes, it requires extra list manipulation, but it makes list manipulation on affected hexes easier.

When creating a hex with embedded lists, you firstly create the list that will contain the embed, and then you create the list that will be embedded within the first one. Remember; even without nesting Introspection and Retrospection, you still need to draw [Consideration](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us/#patterns/patterns_as_iotas@hexcasting:escape) twice to insert it once into the list.

Embedded lists get highlighted brackets within base Hexcasting! They also only count as 1 list element, and therefore do not bloat indices nearly as much as nested introspection and retrospection, making it easier to accomplish any embedding. They do make it more difficult to use hex editing macros, which prefer "flat" lists, however you might find manual editing simpler this way. A common mantra would be:
```k
Numerical Reflection: index of edited list
Dioscuri Gambit
Selection Distillation
//do your list editing now
Surgeon Exaltation
```
to take out an embedded list in one piece in order to modify it, and immediately prepare the original containing list to embed the modified list back in its place. This is more complicated if you just needed to switch out a misdrawn pattern, but it is easier if you needed to redraw the whole embedded list, or add a few patterns somewhere.

Additionally, [[Quines]] already use embedded lists! The Consideration-using Quine with a leading payload looks like the following:
```k
[
	\[Payload hex]
	Hermes' Gambit
	\[Inner copy]
	Numerical Reflection: 4
	Prospector's Gambit
	Surgeon's Exaltation
]
```
Between the outer copy and the inner copy, this quine takes up 8 fewer patterns. At the cost of 2 extra patterns, you could also embed the number to reduce the evaluation overhead to 3 per cast!