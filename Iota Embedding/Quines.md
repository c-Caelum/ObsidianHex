## Idea
Quines are very simple in theory. You use [[Iota Embedding]] to embed a copy of a hex into itself, and then that hex now has the blueprint to do it again. The nuances, though, are a little more complicated. 

So first, we start with our [[Iota Embedding]] method of choice. The placeholder will be replaced with the hex, which is how I will refer to it from now on. Then, we get the index of the placeholder (through [Locator's Distillation](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us#patterns/lists@hexcasting:index_of) or a normal [Numerical Reflection](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us#patterns/numbers@Numbers)), and then pull up the hex (typically using [Prospector's Gambit](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us#patterns/stackmanip@hexcasting:over)) and use [Surgeon's Exaltation](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us#patterns/lists@hexcasting:replace) to replace the placeholder in that hex. Then, that hex can repeat that, and now you have a *Quine*.

Technically, all a quine does is return its source code. Often, however, you will see it used in loops, as it is incredibly useful for keeping the stack clean and looping efficiently. 

## Further research
[Hexcasting Quine Animation](https://www.youtube.com/watch?v=OVn7P1Y4lTs) by chloe, shows how the stack progresses as time goes on.


