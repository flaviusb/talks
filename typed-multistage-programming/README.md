# Typed Multi-Stage Programming

## What is the motivation

- Type safe code generation
- Stage fragment manipulation
- Teasing out what is meant by a 'stage'
  * Typically, used like macros
  * But can also talk about build pipeline, CI, the 'type' of your test suite, dependency management, deployment...
  * Yes, deployment. 'Mobile code' was an old idea (ref emerald etc) but modern interpretations use modol types to encode 'location'
  * There are also temporal modalities; we can use those to represent traditional 'macro like' multistage programming.
  * With effects and coeffects we can express the environment that code needs to be generated for/run in


## References

Emerald - early mobile code as 'active objects'
Build Systems à la Carte - that hypothetical matrix of build system possibilities paper, which introduces Shake
modal types for mobile code - thesis about what it says on the tin
two-dimensional modal logics ...
modal quantification ...
polymorphic row types ...
various oleg papers about effects and coeffects...
entropic - (federated package management) https://github.com/entropic-dev/entropic/tree/master/docs#apis
dataflow languages like oz/mozart
