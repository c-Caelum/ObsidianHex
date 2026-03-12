## The problem
Let's say you wish to create a hex for [Greater Teleportation](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us/#patterns/great_spells/teleport). You might know the coordinates of your home base, but it'd be very difficult to recreate them on the spot using vector math! Lugging around an extra focus just for your coordinates is not convenient either. How do we solve this? Luckily, there's a technique that can help: *iota embedding*.

## Why can't we just *put it there*?
If we want to get an iota on the stack, why can't we just *put it there*? Well, it's because [Hermes' Gambit](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us#patterns/meta@hexcasting:eval) doesn't take kindly to things other than patterns being executed, presenting you with a mishap. To get around this, we "*[escape](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us#patterns/patterns_as_iotas)*" the pattern, which is a scary term for "stop it from executing." [[Screenshot 2026-03-12 204911.png]]

## Option One
Do you remember [introspection](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us#patterns/patterns_as_iotas@hexcasting:open_paren) and [retrospection](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us#patterns/patterns_as_iotas@hexcasting:close_paren)? Everything between them is pushed, as a list, to the stack. This is important because, well, it doesn't *care* about the things between them. It pushes them as a list, period. This means we can shove other iotas in there, that aren't necessarily patterns, pushing them as a list. We can then use [Flock's Disintegration](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us#patterns/lists@hexcasting:splat) to unpack this list, placing the contents onto the stack.
Unfortunately, Flock's Disintegration uses an evaluation, which is why you might use option two.
More on *getting the iota in there* at the end.

## Option Two
[Consideration](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us#patterns/patterns_as_iotas@hexcasting:escape) pushes the next iota after it to the stack. It, once again, does not *care* about whether it is a pattern or an iota. This option is nice because it doesn't use an evaluation. *However*, if you use this in tandem with introspection and retrospection at all, you will have to use 2^n considerations, where n is the depth of introspection and retrospection. So, logically, we just need to get the iota in there. Now, how do we do that?

## Getting the iota in there
I will forever reiterate that **hexes are data**. They are lists, nothing more. Notably, this means that you can use [List Manipulation](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us#patterns/lists) to edit them! More specifically, [Surgeon's Exaltation](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us#patterns/lists@hexcasting:replace) is of use here, as we can just find the index of where we want to put the iota, and then do surgery to put it in there. Because Surgeon's Exaltation requires a thing to replace, we use placeholders to signify where the iota will go. A common one is [Bookkeeper's Gambit: -](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us#patterns/stackmanip@hexcasting:mask), but do note that this is just convention, and any placeholder will do.

[[Quines]] are also a very useful application of iota embedding, so go learn those next if you're ready!

Suggest changes by making an issue, PR, or by pinging me on discord. Thank you for reading!
