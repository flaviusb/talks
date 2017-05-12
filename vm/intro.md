VM

Reasoning
- A 'fixed point', a 'simple core', 'portable' (in various different senses), 'a project to hang other experiments off of, as production VMs are not built with that in mind'

Prior Art
- ZetaVM (https://pointersgonewild.com/2017/04/29/zetavm-my-new-compiler-project/)
- Qbrt (http://strange-loop-2013-notes.readthedocs.io/en/latest/emerging-languages/qbrt.html)
- Truffle/Graal?
- IBM's new java and Eclipse based abomination
- Parrot
- Even Forth, kindof
- Lots of stuff

Average longevity: low. Many of these projects quickly die. Why?

A basic bytecode VM is the simplest program
- An array of bytes
- Stack(s), register(s) of some kind
- An instruction counter
- A dispatch loop based off the contents of the array indexed by the instruction counter

Basically a reification of the command pattern as a coprogram.

But 'productising' a VM is quite hard. Lacking a specific reason to exist, (and a sponsor with deep pockets) they usually vanish.

...

Refined reasoning
- 'A better bootstrapping point than C/C++'
  - Not having to deal with C/C++
  - Stability through time
  - 'portability' by means of a simple enough spec to reimplement, plus having simple compilers written in the bytecode for the bytecode various modern environments

The approach
ie 'Why is this interesting and new, if it is so boring and old'
- Different primitives
  - Typed
  - Polymorphic row types
  - Logical quantifiers (μ λ ∃ ∀ ı)
  - Reasonably developed MOP
  - GOT etc are part of the VM spec, and are accessible
- Some other assumptions are taken in very different ways
  - Character encodings/strings
  - Traces/Basic Block Versions as a part of the function ABI/structure in the VM spec, not something layered on as an optimisation
    - This has a similar impact to mandating TCO in the language spec, in that it makes certain things guaranteed to be feasable
    - But, because this is under program control it also ends up with a very different approach to eg Meta-Tracing

The details together lead in very different directions
- We can get functorial modules
  - Quantifiers + MOP
  - No 'classloader shenanigans' or w/e to have different versions of the 'same' package, namespaces, unloading etc

More prior art
- The Interpreter Tower, Kuro/Shiro, Intercession etc
- Metatracing VMs
- Staging/staged computation
