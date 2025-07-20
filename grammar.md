## Grammar

### Description

The first part of this file states *Calcit* in *Backus–Naur Form*[^1]. The second part describes it as grammar for the [Peggy](https://peggyjs.org/) parser generator.

### Definition

```
<compound-query> := <query> (<whitespace>? <compound-operator> <whitespace>? <query>)*

<whitespace> := (" " | \t)* \n? (" " | \t)*   // You cannot use more than one consecutive newline to split up queries.

<line-break> := (" " | \t)* \n (" " | \t)*

<compound-operator> := "|" | "&" | "~"

<query> := <tag-start> | <node-start>

<tag-start> := <tag> (<omit-symbol>? <arrow-left> <omit-symbol>? <node-start>)?

<node-start> := <node> ((<omit-symbol>? <arrow-right> <omit-symbol>? <tag-start>) |
                        (<omit-symbol>? <arrow-left>  <omit-symbol>? <node-start>) |
                        (<omit-symbol>? <arrow-right> <omit-symbol>? <node-start>) |
                        (<omit-symbol>? <link> <omit-symbol>? <node-start>))?

<link> := <link-left> | <link-right>

<inner-join>

<left-join>

<right-join>

```

...

### Peggy

...


[^1]: For simplicity and beauty I'm using the variant `:=` instead of `::=`. `*` means 0 or more occurrences. `+` means 1 or more occurrences. `?` means 0 or 1 occurrences. Parentheses are used for grouping. https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_form. 
