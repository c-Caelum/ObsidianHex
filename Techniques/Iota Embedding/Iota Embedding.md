---
author: Caelum, Matt6049
tags:
  - IotaEmbedding
  - Consideration
  - IntroRetro
---
## The problem
Let's say you have an iota which is difficult or impossible to produce at runtime. If you need this iota, you might feel locked in to using a focus, and carrying that around for the rest of your life. Luckily, there's a technique that can help us here: *iota embedding*. There are many different ways to do this, but here's the first two:

## Option One
Do you remember [introspection](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us#patterns/patterns_as_iotas@hexcasting:open_paren) and [retrospection](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us#patterns/patterns_as_iotas@hexcasting:close_paren)? Everything between them is pushed, as a list, to the stack. This is important because, well, it doesn't *care* about the things between them. It pushes them as a list, period. This means we can shove other iotas in there, that aren't necessarily patterns, pushing them as a list. We can then use [Flock's Disintegration](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us#patterns/lists@hexcasting:splat) to unpack this list, placing the contents onto the stack.

![An embedding example.|192](https://github.com/c-Caelum/ObsidianHex/tree/main/Techniques/Iota%20Embedding/images/embedding1.png)

This is the method that is the easiest to deal with, however it is quite bulky, requires an extra evaluation (Flock's Disintegration), and can be less readable than the alternative. It also does not have any particularly weird techniques, although those tend to be quite niche.
## Option Two
[Consideration](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us#patterns/patterns_as_iotas@hexcasting:escape) pushes the next iota after it to the stack. It, once again, does not *care* about whether it is a pattern or an iota. This option can at times be more desirable because it is smaller, doesn't use an [evaluation](Casting%20Mechanics/Niche/Evaluation%20Limit), and allows for [certain techniques](Consideration%20Overview.md) that aren't as neat with Introspection and Retrospection. It is however more difficult and less convenient to use.

**Consideration needs to be doubled up inside an Introspection/Retrospection block. 
Blue Consideration - not in the list yet;
Yellow Consideration - is in the list.

The above necessitates a change in approach - instead of nesting Introspection and Retrospection, you should use list embeds. This prevents this issue from stacking and has some other advantages, with the biggest disadvantage being the inconvenience of having to create an additional embed. 

So, logically, we just need to get the iota in there. Now, how do we do that?
## Getting the iota in there
I will forever reiterate that **hexes are data**. They are lists, nothing more. Notably, this means that you can use [List Manipulation](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us#patterns/lists) to edit them! More specifically, [Surgeon's Exaltation](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us#patterns/lists@hexcasting:replace) is of use here, as we can just find the index of where we want to put the iota, and then do surgery to put it in there. Because Surgeon's Exaltation requires a thing to replace, we use placeholders to signify where the iota will go. A common one is [Bookkeeper's Gambit: -](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us#patterns/stackmanip@hexcasting:mask), but do note that this is just convention, and any placeholder will do.


```patterns
{
	{
		Bookkeeper's Gambit: -
	}
	Flock's Disintegration
	\\Bookkeeper's Gambit: -
}
```
Remember that you need to draw Consideration twice to insert it into a list, even while not nesting Introspection and Retrospection.

[[Quines]] are also a very useful application of iota embedding, so go learn those next if you're ready!

Suggest changes by making an issue, PR, or by pinging me on discord. Thank you for reading!
