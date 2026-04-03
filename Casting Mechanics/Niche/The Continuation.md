---
author: IridescentVoid
---

For a better viewing experience, copy-paste the raw markdown into Obsidian.

## The Jump Iota
[Iris' Gambit](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us/#patterns/meta@hexcasting:eval/cc) differs from [Hermes' Gambit](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us/#patterns/meta@hexcasting:eval) in only one way: it places a "Jump" iota on the stack before executing the pattern list. When evaluated (using Hermes' Gambit), the jump iota skips to the end of the pattern list, omitting execution of any remaining patterns. This is useful for exiting nested executions of Hermes' Gambit and [Thoth's Gambit](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us/#patterns/meta@hexcasting:for_each), since [Charon's Gambit](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us/#patterns/meta@hexcasting:halt) can only skip to the end of the closest Hermes' or Thoth's cast, and not beyond.

The book notes:
> The "Jump" iota will apparently stay on the stack even after execution is finished... better not think about the implications of that.

So, what are the implications of that?

First, we'll observe the behaviour of executing a Jump iota that is left on the stack, after Iris' Gambit has finished running the pattern list.

```patterns
Introspection
    Introspection
        Numerical Reflection: 1
        Numerical Reflection: 2
        Additive Distillation
    Retrospection
    Iris' Gambit
    Reveal
    Bookkeeper's Gambit: v
Retrospection
# [ Introspection, Numerical Reflection: 1, Numerical Reflection: 2, Additive Distillation, Retrospection, Iris' Gambit, Reveal, Bookkeeper's Gambit: v ]

Hermes' Gambit

(3.0 is revealed in chat)
# [Jump]
```

Let's put something else on the stack, for reasons that will become more clear later. Then evaluate the jump iota:
```patterns
Numerical Reflection: 5
# [Jump], 5.0

Jester's Gambit
# 5.0, [Jump]

Herme's Gambit

(5.0 is revealed in chat, and nothing is left on the stack)
```

If you look at the original pattern list we made, you'll notice that the Iris' Gambit was followed by a Reveal and a Bookkeeper's Gambit: v. When we executed the jump iota, Reveal was casted, followed by Bookkeeper's Gambit: v. So, jump iotas can take us back in time.

> [!info] Jump iotas are regular iotas
> They can be manipulated like any other iota, including being duplicated by [Gemini Decomposition](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us/#patterns/stackmanip@hexcasting:duplicate) and being placed in lists. They can even be written to a permanent medium using [Scribe's Gambit](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us/#patterns/readwrite@hexcasting:write)! (Note that Jump iotas are supposed to take up stack space based on how much information is stored within - there is a bug in currently released versions of Hex Casting that make it take much less space.)

## Pulling Back The Curtains

Understanding how jump iotas work behind-the-scenes is necessary for using them effectively. Let's look at how Hex Casting evaluates a pattern list, such as when you use a [Cypher, Trinket, or Artifact](https://hexcasting.hexxy.media/v/0.11.3/1.0/en_us/#items/hexcasting). (Casting with a wand is just a special case of evaluating a pattern list of length one.^[`queueExecuteAndWrapIota` in [`CastingVM`](https://github.com/FallingColors/HexMod/blob/main/Common/src/main/java/at/petrak/hexcasting/api/casting/eval/vm/CastingVM.kt)])

References to source code will be included as footnotes. This is optional further reading if you want to understand more of how Hex Casting works under the hood, but not necessary to understand this article.

---

During pattern list evaluation, the Hex Casting interpreter has to keep track of what patterns to run next and a bit of information about what has been executed, such as where a Charon's Gambit should go (since it ends the nearest Hermes' or Thoth's Gambit), or for a Thoth's loop, what the original stack was (in order to restore it at the start of each iteration) and what the currently accumulated output entries are. Since meta evaluations can be nested, we also need a nested structure for storing this information.

Enter the Continuation Frame system. Different meta-evaluations push different frames, which track the information listed in the second paragraph. These continuation frames form a stack structure (separate from your iota stack, and not directly manipulable via hex without addons) where the topmost frame represents information about the meta-evaluation and patterns currently being processed. ^[[`ContinuationFrame`](https://github.com/FallingColors/HexMod/blob/main/Common/src/main/java/at/petrak/hexcasting/api/casting/eval/vm/ContinuationFrame.kt)s are stored in a singly linked list of [`SpellContinuations`](https://github.com/FallingColors/HexMod/blob/main/Common/src/main/java/at/petrak/hexcasting/api/casting/eval/vm/SpellContinuation.kt)]. This stack of frames is separate from your iota stack, and not directly manipulable using hex. For the rest of this article, "stack" refers to the continuation frames unless specified as the "iota stack".

At the start of a pattern list evaluation, a [`FrameEvaluate`](https://github.com/FallingColors/HexMod/blob/main/Common/src/main/java/at/petrak/hexcasting/api/casting/eval/vm/FrameEvaluate.kt) is placed at the top of stack, containing the list of patterns to be run. From then on, the topmost frame is popped off of the stack and its `evaluate` action is executed until no frames are left on the stack or a mishap occurs.

Note that the iota stack and other environment state (such as whether the next pattern evaluated should be escaped by Consideration or Introspection) is kept separately from the frame stack^[see [`CastingImage`](https://github.com/FallingColors/HexMod/blob/main/Common/src/main/java/at/petrak/hexcasting/api/casting/eval/vm/CastingImage.kt)].

## FrameEvaluate

`list` - the list of patterns (remaining) to be evaluated

If `list` is empty then there is nothing to evaluate, and so `evaluate` simply returns. (Since the frame is popped from the stack before evaluation, this means the frame below it gets to run.)

Otherwise:
If `list` contains more than one pattern, push a new FrameEvaluate with a copy of `list` from the second entry onwards. This queues execution of the remaining patterns.
Then, take the first entry of `list` and execute its behavior.

## Hermes' Gambit^[See [`OpEval`](https://github.com/FallingColors/HexMod/blob/main/Common/src/main/java/at/petrak/hexcasting/common/casting/actions/eval/OpEval.kt)]

`patterns` - the pattern or list of patterns passed to Hermes' Gambit for evaluation

The following occurs during the "execute its behavior" part of `FrameEvaluate`'s execution.
If `patterns` is a list of patterns, push a `FrameFinishEval`. That frame marks where Charon's Gambit should stop when it is aborting execution.
Then, push a `FrameEvaluate` with `patterns` as its list of patterns to be evaluated. (If only one pattern was given, it is wrapped in a list.)

## FrameFinishEval and Charon's Gambit

Denotes a 'boundary' that Charon's Gambit should stop at^[Specifically, `breakDownwards` in [`FrameFinishEval`](https://github.com/FallingColors/HexMod/blob/main/Common/src/main/java/at/petrak/hexcasting/api/casting/eval/vm/FrameFinishEval.kt) returns a `Pair` with the left boolean being `true`.]. When evaluated, the frame does nothing. (And is automatically popped off of the stack.)

Charon's Gambit^[See [`OpHalt`](https://github.com/FallingColors/HexMod/blob/main/Common/src/main/java/at/petrak/hexcasting/common/casting/actions/eval/OpHalt.kt)] operates by repeatedly popping frames off the stack until it either encounters a `FrameFinishEval` (which it will discard before stopping), or runs out of frames to pop. Thus, if it is encountered in a pattern list during a Hermes' execution, it'll first pop the `FrameEvaluate` containing the remaining patterns for the Hermes' Gambit evaluation, then pop the `FrameFinishEval`. Then it stops, leaving any `FrameEvaluate`s below intact and ready to execute the remaining instructions.

## An Example

Take the following patterns:
```patterns
Introspection
    Introspection
        # some list of patterns. the bookkeeper's gambit is a placeholder
        Bookkeeper's Gambit: -
    Retrospection
    Hermes' Gambit
    Mind's Reflection
Retrospection
Hermes' Gambit
```

Let's walk step by step through running the last Hermes' Gambit.

1. `FrameEvaluate(list = [Herme's Gambit])`:  Since the list contains one pattern, `FrameEvaluate` does not push anything. Then, we execute Hermes' Gambit's behavior.
  - Since `patterns` is a list of patterns, push a `FrameFinishEval`. Then, push `FrameEvaluate` with `patterns` as the list.
  - At the end of this evaluation step, the frame stack (from top to bottom): `FrameEvaluate(list = [Introspection, ..., Retrospection, Hermes' Gambit, Mind's Reflection])`, `FrameFinishEval`.
2. `FrameEvaluate(list = [Introsection, ...])`: A new `FrameEvaluate` is made and pushed to the stack, with the `list` containing everything apart from the first Introspection. We execute the Introspection's behavior, which sets a flag to consume all iota until a Retrospection is encountered^[Introspection, Retrospection, Consideration, and Evanition are actually handled by the `CastingVM` directly in `handleParentheses`, which is called by `executeInner`. It also tracks the number of introspections and retrospections encountered to support nesting, in `CastingImage`'s `parentCount`.].
  - At the end of this evaluation step, the frame stack: `FrameEvaluate(list = [..., Retrospection, Hermes' Gambit, Mind's Reflection])`, `FrameFinisheEval`.
3. `FrameEvaluate(list = [..., Retrospection, ...])`: For subsequent evaluations, the iota is stored as part of the "list currently being built"^[`CastingImage`'s `parenthesized` property] rather than being evaluated. Eventually, we use up all of the patterns between the Introspection and Retrospection.
  - At the end of these evaluation steps, the frame stack: `FrameEvaluate(list = [Retrospection, Hermes' Gambit, Mind's Reflection])`, `FrameFinishEval`.
4. `FrameEvaluate(list = [Retrospection, ...])`: A new `FrameEvaluate` is made and pushed to the stack. Retrospection is processed, which now pushes a pattern list to the iota stack.
  - At the end of this evaluation step, the frame stack: `FrameEvaluate[Hermes' Gambit, Mind's Reflection])`, `FrameFinishEval`.
5. `FrameEvaluate(list = [Herme's Gambit, ...])`: A new `FrameEvaluate` is made and pushed to the stack, with `list` containing the Mind's Reflection. Then, we execute Hermes' Gambit's behavior.
  - Since `patterns` is a list of patterns, push another `FrameFinishEval`. Then, push `FrameEvaluate` with the patterns as the list. (We'll use Bookkeeper's Gambit: - as the pattern list for this example.)
  - At the end of this evaluation step, the frame stack (from top to bottom): `FrameEvaluate(list = [Bookkeeper's Gambit: -])`, `FrameFinishEval` (from the Hermes' Gambit we're executing now), `FrameEvaluate(list = [Mind's Reflection])` (the remaining patterns after the Hermes' Gambit), `FrameFinishEval` (from step one).
6. `FrameEvaluate(list = [Bookkeeper's Gambit: -])`: Since the pattern list only has one iota, no new `FrameEvaluate` is pushed. `Bookkeeper's Gambit: -` does nothing (we'll assume there's some other iota on the iota stack so that it doesn't mishap.)
  - The frame stack now: `FrameFinishEval`, `FrameEvaluate(list = [Mind's Reflection])`, `FrameFinishEval`.
7. `FrameFinishEval`: Does nothing.
  - The frame stack now: `FrameEvaluate(list = [Mind's Reflection])`, `FrameFinishEval`.
8. `FrameEvaluate(list = [Mind's Reflection])`: Only one iota, so no new `FrameEvaluate` is pushed. Mind's Reflection is evaluated.
  - The frame stack now: `FrameFinishEval`.
9. `FrameFinishEval`: Does nothing. Frame stack is left empty.
10. Execution ends, since there are no more frames on the frame stack.

## Iris' Gambit^[See [`OpEvalBreakable`](https://github.com/FallingColors/HexMod/blob/main/Common/src/main/java/at/petrak/hexcasting/common/casting/actions/eval/OpEvalBreakable.kt)] and ContinuationIota

When evaluated, it copies the current frame stack and wraps it in an [`ContinuationIota`](https://github.com/FallingColors/HexMod/blob/main/Common/src/main/java/at/petrak/hexcasting/api/casting/iota/ContinuationIota.java). Then, it calls the same logic as Hermes' Gambit, which pushes a `FrameFinishEval` (if necessary) and the `FrameEvaluate` for the provided pattern list.

The `ContinuationIota` is what the Hex Notebook calls the "Jump" iota. It contains a snapshot of the frame stack. When it is evaluated^[See `execute` on `ContinuationIota`], it *replaces* the current frame stack with the contents of the snapshot. (The iota stack is not touched, because it is kept indepdently of the frame stack.)

## Thoths' Gambit and FrameForEach

TODO: write this section

TODO: add note on getting overevaluate to see jump iota contents and for more metaevaluation/continuation frame manipulation