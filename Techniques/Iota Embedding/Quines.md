## Idea

Quines are simply hexes (or even programs!) that return themselves. Initially, this may seem like a paradox; how can a hex contain itself? However, as you'll see, quines are perfectly possible and very useful in hex.

To give an analogy, many quines in hex are like using two tiles and picking up the old one to keep going forward, using [[Iota Embedding]] to "pass the tile on." By embedding a copy of itself into itself, it sends the iota "into the future," or into the next execution. Now, that hex, when executed, has the blueprint to redo the process. This is what forms a quine.

So first, we start with our [[Iota Embedding]] method of choice. The placeholder will be replaced with the hex, which is how I will refer to it from now on. Then, we get the index of the placeholder (through [Locator's Distillation](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us#patterns/lists@hexcasting:index_of) or a normal [Numerical Reflection](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us#patterns/numbers@Numbers)), and then pull up the hex (typically using [Prospector's Gambit](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us#patterns/stackmanip@hexcasting:over)) and use [Surgeon's Exaltation](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us#patterns/lists@hexcasting:replace) to replace the placeholder in that hex. Then, that hex can repeat that, and now you have a *Quine*.

![[quine1.png]]

It's very important that I emphasise that the copying part itself can be *anywhere* in the hex, so long as the index (the [Numerical Reflection](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us#patterns/numbers@Numbers) in the hex) is updated accordingly. For example, you can have it at the end:

![[quine2.png]]

...Or wherever it's useful to you! Another important thing is that quines need no external input. Compared to a Gemini-Hermes loop, quines are clean and self-contained on the stack.

Technically, all a quine does is return its source code. Often, however, you will see it used in loops, as it is incredibly useful for keeping the stack clean and looping efficiently. 

## Further research
[Hexcasting Quine Animation](https://www.youtube.com/watch?v=OVn7P1Y4lTs) by Chloe, shows how the stack progresses as time goes on.
[Quine - Wikipedia](https://en.wikipedia.org/wiki/Quine_(computing)) by Wikipedia, shows how ~~quines were named after an old geezer~~ quines are defined in the context of computing.

