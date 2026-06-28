---
tags:
  - Consideration
  - IotaEmbedding
  - Continuations
  - StackManipulation
author: Matt6049
---
Have you ever wanted to send an iota into the future? Has a necessary iota ever bothered you by lying around on the stack for decades before it could actually be used? What if you could instead make that iota disappear off your stack, and reappear only when you actually need it? This can be achieved with Future [[Iota Embedding|Embedding]].

## What the hell is Future Embedding?
Future Embedding is simultaneously a very simple and decently complicated concept. Instead of embedding some unchanging value during a hexes creation, we take some value generated during runtime, and embed it at the end of what we were about to evaluate. Consider this example:
```k
\[
	Numerical Reflection: 1
	Huginn's Gambit
	Muninn's Reflection
	Reveal
	Bookkeeper's Gambit: v
]
Hermes' Gambit
```
This hex pretty clearly overwrites the [Ravenmind](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us/#patterns/readwrite@hexcasting:local). But what if we had some important value on the Ravenmind that we wanted to preserve? There is a solution, and it starts with adding a **Trailing Consideration** at the end! Let's modify this hex to preserve our old Ravenmind state.

```k
\[
	Numerical Reflection: 1
	Huginn's Gambit
	Muninn's Reflection
	Reveal
	Bookkeeper's Gambit: v
	Consideration Consideration //note how this currently has no embed! 
]
Muninn's Reflection      //load up the old Ravenmind state
Integration Distillation //NOW we're adding the embed. 
						 //We're not doing this manually; this is part of the hex!
Hermes' Gambit
Huginn's Gambit
```
The old Ravenmind state should re-appear after we're done casting that inner hex with Hermes' Gambit. We can then immediately consume that iota by putting it back on the Ravenmind. In the meantime, it's not lingering anywhere on the stack. 

## Pseudoquines
In addition to stack manipulation, this can be used for craft something called a **Pseudoquine** (see: [[Quines]]). It's just a specific case of Future Embedding, and can be treated like a Leading Payload Quine that requires external assistance for copying. It can be useful if you still need to preserve a hex after casting and don't want it lingering on the stack (like it would be had you simply cast `Gemini Decomposition; Hermes' Gambit`), but also do not want to assemble a full quine for the sake of this task, which would potentially take up a lot of space within your focus.

Let's use a simple mining hex as an example:
```k
\[
	Mind's Reflection
	Compass Purification
	Mind's Reflection
	Alidade's Purification
	Archer's Distillation
	Break Block
	Consideration //trailing consideration again! 
				  //This is necessary for a pseudoquine.
]
Gemini Decomposition
Integration Distillation
Hermes' Gambit
```
The last three are effectively the Pseudoquine mantra. If the hex contains a trailing consideration, this will future embed a copy of the hex, resulting in pushing a fresh copy of the hex upon casting it.
***Note:** such hexes will mishap if you do not future embed anything in them.*
## Where is this iota actually stored?
Embeds can be seen as a form of [[The Continuation|Continuation Stack]] manipulation. As explained in the linked note, whenever you cast anything, it gets pushed onto there before evaluation. That means that [[Iota Embedding]] is the process of moving an iota from the Iota Stack onto the Continuation Stack! When the embed gets evaluated, this process gets reversed; the iota gets popped from the Continuation Stack, and pushed onto the Iota Stack.
