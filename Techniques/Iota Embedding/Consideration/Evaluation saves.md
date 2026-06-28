---
tags:
  - Consideration
  - IotaEmbedding
author: Matt6049
---
[[Evaluation Limit|Evaluation]] saves are the reason you might still choose to sometimes use [Consideration](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us/?nospoiler#patterns/patterns_as_iotas@hexcasting:escape) [[Iota Embedding|embeds]] as an [Introspection](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us/?nospoiler#patterns/patterns_as_iotas@hexcasting:open_paren) and [Retrospection](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us/?nospoiler#patterns/patterns_as_iotas@hexcasting:close_paren) user. This is however a niche use that finds little application outside of extreme, eval-hungry circumstances.

## Core concept
[Escape patterns](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us/#patterns/patterns_as_iotas) do not use up evaluations. As a result, Consideration embeds do not use up any evaluations at all, unlike Introspection and Retrospection which generally requires [Flock's Disintegration](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us/#patterns/lists@hexcasting:splat).

In a normal hex, this might not have much of an effect. Where this shines, however, are loops (link here later). Large loops (like quarries, for example) tend to be incredibly evaluation heavy, and might even require several ticks to fully process!

## How to cheese this
Right, but wouldn't that only help if the loop uses a lot of embeds? That is correct, but we can abuse this to our advantage. Do you know what uses up evaluations? [Numerical Reflections](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us/#patterns/numbers@Numbers), [constants (including Vector Reflection!)](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us/#patterns/consts), even [Mind's Reflection](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us/#patterns/basics@hexcasting:get_caster) can be turned into an embed! Turning the numerical reflections for a [[Quines|Quine's]] [Surgeon's Exaltation](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us/#patterns/lists@hexcasting:replace) into embeds can save an evaluation, potentially reducing the evaluation overhead down to 2! 