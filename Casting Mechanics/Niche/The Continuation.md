## The Idea
Internally, Hexcasting stores each hex to be executed as a "stack" of things to be executed. This is called the continuation. 

## Details
Items on the continuation are called "continuation frames", or just frames for short. Different frames do different things.
## Types of Frames
### FrameEvaluate
FrameEvaluate does what you'd think it does. It executes a hex. Hermes' Gambit pushes one of these to the continuation, which is what does the actual executing

### FrameFinishEval
This works as a marker for [[Charon's Gambit]] to exit from. It does nothing of importance except marking this.

### FrameForEach
FrameForEach also does what you'd think it would do. It executes (using [[#FrameEvaluate]]) a hex for every element in a list, and adds that to the accumulator (term for the list Thoth's pushes at the end). Obviously, this is used for Thoth's Gambit.