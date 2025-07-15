## Grammar

### Description

The first part of this file states *Calcit* in *Backus–Naur Form*[^1]. The second part describes it as grammar for the [Peggy](https://peggyjs.org/) parser generator.

### Definition

```
<compound-query> := <query> (<whitespace> <compound-operator> <whitespace> <query>)*

<whitespace> := (" " | \t)* | (\n (" " | \t)*)

<compound-operator> := "|" | "&" | "~"

```

...

### Peggy

...


[^1]: For simplicity and beauty I'm using the variant `:=` instead of `::=`. `*` means 0 or more occurrences. `+` means 1 or more occurrences. `?` means 0 or 1 occurrences. Parentheses are used for grouping. https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_form. 
