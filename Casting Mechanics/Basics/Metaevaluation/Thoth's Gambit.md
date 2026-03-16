
## The Basic Idea
If you've done programming before, Thoth's Gambit is comparable to a `for` loop.  For each element in the list on the top of the stack, it runs the hex second from the top (imagine [[Hermes' Gambit]]), starting with one of the iotas on the stack. Whatever you leave on the stack is pushed to the resulting list

## Nuances
Thoth's gambit makes a copy of the stack for each element. This means that if you don't clear it up, you'll have a lot of unneeded iotas. 

