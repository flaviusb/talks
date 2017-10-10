VM

Bytecode VMs.

Rough kinds: (This will be important in a minute)
- For a specific language
  - Python CPython, Ruby MRI/Rubinius, Perl, Lua, Flash Tamarin, JS V8 etc, even PHP (kindof)
  - (Usually) an evolution, after starting as (usually) an AST walking interpreter
  - Better risk/reward than writing a compiler that targets raw machine code
- As a layer of an application or part of an API
  - Z-machine (Infocom)
  - The Lost Vikings
  - JS rendering library 'Glimmer'
  - I wrote an opengl based UI toolkit which was mostly a bytecode VM whose bytecodes were rendering tasks
  - In fact, lots of rendering middleware uses this approach - Apple's Metal APIs, for example.
  - SPIRV for Vulkan is even more this.
  - If you are familiar with the literature, you can end up building out Jitting infrastructure, GC, OSR, Deopt, multithreading, tuning parameters, stack layout optimisation, playing with instruction encoding for code density, macroinstructions, polymorphic inline caches, dynamic recompilation...
- 'General' runtimes
  - JVM, CLR, Parrot, Truffle/Graal, OMR, ZetaVM, Qbrt, Rubinius, PyPy, Dis, Mu VM...

What is the underlying idea here?

Basically a reification of the command pattern as a coprogram.

A basic bytecode VM is the simplest program
- An array of bytes
- Stack(s), register(s) of some kind
- An instruction counter
- Some other state variables
- A dispatch loop based off the contents of the array indexed by the instruction counter
  - Basically a loop with a giant switch statement inside

Well, for some things that is as far as you need to go - you can now express what you want to do in the form of simple bytecode programs, which, if you are clever, can have some very nice properties, such as easy programmatic transformation. You don't need GC, or high performance, or low memory use, or threads, or lots more features.

But ... big wodge of C++.

... A floor wax, a dessert topping - sometimes it is a design pattern, sometimes it is a product category. So...?

So, I wrote a VM. Am writing? Whatever. Some questions: why? Who cares? What is interesting about it?

Reasoning in short
- A 'fixed point', a 'simple core', 'portable' (in various different senses), 'a project to hang other experiments off of, as production VMs are not built with that in mind'
Note: 'a stable place to stand *for me*'; no guarantee of external stability

Prior Art
- ZetaVM (https://pointersgonewild.com/2017/04/29/zetavm-my-new-compiler-project/)
- Qbrt (http://strange-loop-2013-notes.readthedocs.io/en/latest/emerging-languages/qbrt.html)
- Truffle/Graal
- IBM's OMR, a new java and Eclipse based abomination (https://projects.eclipse.org/proposals/omr)
- Parrot, MoarVM, PyPy
- Even Forth, kindof
- Lots of stuff

Average longevity: low. Many of these projects quickly die. Why?


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
    - Homotopy type theory, cubical types ish
    - All a little bit time cube
  - Polymorphic row types
    - labelled sums and products with first class labels
    - polymorphic extension, restriction, union, and removal
  - Logical quantifiers (∀ ∃ Π Σ ı λ μ ν Ϙ 𐌎)
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

Finally, the bytecode, execution model, and environment were all designed before the 'vm'.
- Designed to be small and easy to implement
- That is, possible to 'port' by just writing a whole new conformant VM, and then carrying everything else atop it
- Everything designed to make staged computation and metatracing easier to write

Why did I chose the primitives I chose?
A couple of reasons.
The ML family are really good for building large chunks of compilers - a lot of this is gadts + pattern matching - the AST typing problem, plus parts of the expression problem.
Generally they are only so-so for building the string processing side.
Polymorphic Row Types are basically the apotheosis of the good bits of this, and they require relatively few, relatively simple primitives to build.

Stability through 'deep time'
Even just stability through short periods of time
I have written a number of languages - the very first one that was used in production was a language called Atomish, that was like a compatible superset of a language called Ioke. I wrote it because Oracle released a patch level release for the JDK, which Ioke ran on top of, and then Ioke stopped working forever.  The patch notes for the release were (paraphrasing) 'minor changes', but they broke a lot of unrelated stuff. The original author of Ioke had moved on to other projects, and was uninterested in getting it working again; I spent something like a month on it, but I could never get it to the point where I could get even the low level diagnostics working, and it was a monster of a project. So, it ended up being easier to write a new, similar language, and just port my entire companies codebase to that.

I wrote Atomish in Scala; my last commits to it were in 2014. It could still build and run as of late 2016; but I am not confident it would even build now. The Atomish spec was informal, huge, and very messy. The semantics were defined by the reference (and only) implementation, which maybe can't even be built anymore. A lot of the acceptance tests were proprietary company code, which is still locked up somewhere. So, what do?

The impermanence of things
Even if your stuff 'keeps working', the dependencies may not. Even if the dependencies do, changes in them may change the behavior of your program.

Languages are pretty big, in a weird way that not many other things are. I have worked on lots of projects; 'real' programming languages are gigantic (in terms of behavioural area) - that is, a minor change to something that would be completely invisible in most projects is a breaking semantic change in a programming language.

So, things rot.

...


Languages are pretty foundational - I want to build a language and a runtime to build stuff on, not just for its own sake.

More prior art
- The Interpreter Tower, Kuro/Shiro, Intercession etc
- Metatracing VMs
- Staging/staged computation
- The STEPS VPRI project with their view of the layers of computation, and translating systems across different towers

Long known that tracing works poorly with interpreters - we seem to not get the almost mythical second Futamura projection... So we have to do something more...
The idea is that we can write an interpreter for this simple bytecode simply, and then write a compiler for the bytecode (a relatively more difficult task) in the bytecode...

Future direction inspirations
- JLink etc for multiple language hosting / ABIs as libraries
