# Meeting 13- Object Orientation

* Javier presenting whole chapter
* Discussion over what OO means
  * SmallTalk devs would say Java isn't OO
    * Because Java has primitives, and doesn't fully implement message-passing
* Got into discussion about the implementation details of `dyn`
  * Non-dynamic method calls are known ahead of time so a bit of code that calls a function knows well ahead of time exactly what function pointer it'll use to call it's function
  * Dynamic method calls don't know that so dynamic objects (trait objects) need to store an extra v-table at run-time that specifies what functions it has and what function pointers are associated with them
  * Video on topic: https://youtu.be/xcygqF5LVmM?t=188
* 
