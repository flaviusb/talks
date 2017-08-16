VM

So, I wrote a VM. Am writing? Whatever. Some questions: why? Who cares? What is interesting about it?

Reasoning in short
- A 'fixed point', a 'simple core', 'portable' (in various different senses), 'a project to hang other experiments off of, as production VMs are not built with that in mind'

Prior Art
- ZetaVM (https://pointersgonewild.com/2017/04/29/zetavm-my-new-compiler-project/)
- Qbrt (http://strange-loop-2013-notes.readthedocs.io/en/latest/emerging-languages/qbrt.html)
- Truffle/Graal
- IBM's OMR, a new java and Eclipse based abomination (https://projects.eclipse.org/proposals/omr)
- Parrot, MoarVM, PyPy
- Even Forth, kindof
- Lots of stuff

Average longevity: low. Many of these projects quickly die. Why?

A basic bytecode VM is the simplest program
- An array of bytes
- Stack(s), register(s) of some kind
- An instruction counter
- A dispatch loop based off the contents of the array indexed by the instruction counter

Basically a reification of the command pattern as a coprogram.

People often write simple bytecode VMs early in the development of their programming languages - they offer a sweet spot of better performance for less effort than continuing to develop direct AST walking interpreters, and a specialised bytecode VM offers a much easier compilation target than machine code, or even translation to C.

But things get complicated fast. Macroinstructions, Polymorphic inline caches, JITting, GC...

- The language bootstrap problem
    - Rust's compiler was originally written in OCaml - eventually they moved to writing in Rust with a bootstrapping compiler saved as a binary blob from the last good version.
    - Guile Scheme, and SBCL both had some interesting problems here as well.
    - You can end up very easily locked into one representation, and it makes ... very hard
    - Certain language changes also become exceptionally painful
    - x86\_64 (or whichever machine language is your primary) eventually becomes a defacto part of your spec - the spec as such is dependent on the meaning of the binary blob that is the exemplar compiler, after all
    - I have tried to resurrect a number of academic languages that have met this fate from the 90's (the major problem with ones from the 80's and earlier is a lack of code at all) - they are often fairly intertwined with eg MIPS or Sparc or whatever, plus other languages that are similarly problematic and emulators are not always good enough to get them running properly again. Let alone the horror of attempting to port them.

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
  - Logical quantifiers (∀ ∃ ı λ μ ∨)
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
