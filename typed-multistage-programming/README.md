#Typed Multi-Stage Programming

##What is the motivation

- Type safe code generation
- Stage fragment manipulation
- Teasing out what is meant by a 'stage'
  * Typically, used like macros
  * But can also talk about build pipeline, CI, the 'type' of your test suite, dependency management, deployment...
  * Yes, deployment. 'Mobile code' was an old idea (ref sapphire etc) but modern interpretations use modol types to encode 'location'
  * There are also temporal modalities; we can use those to represent traditional 'macro like' multistage programming.
  * With effects and coeffects we can express the environment that code needs to be generated for/run in
