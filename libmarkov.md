---
layout: page
title: Libmarkov
permalink: /libmarkov/
---

#A library of sentence generators in different languages

Built in many different languages like Ruby, Golang, and Rust, libmarkov generates dummy text based on [markov chains](https://en.wikipedia.org/wiki/Markov_chain#Internet_applications#Markov_text_generators).  You give the algorithm some training text, something like this

```
I think the best way to tell the story is by starting at the end, briefly, then going back to the beginning, and then periodically returning to the end, maybe giving different characters' perspectives throughout.
```

Then it outputs something like this

```
I think the best way to tell the story is by starting at the end, briefly, then going back to the end, briefly, then going back to the beginning, and then periodically.
```

Current Implementations

+ Javascript (node + clientside)
+ Ruby
+ Golang
+ Rust

Each implementation is well tested, benchmarked, and comes with a library and a stdin/stdout interface.

[You can checkout the library here](https://github.com/jaxgeller/libmarkov)
