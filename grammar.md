## Grammar

### Description

The first part of this file states *Calcit* in *Backus–Naur Form*[^1]. The second part describes it as grammar for the [Peggy](https://peggyjs.org/) parser generator.

### Definition

```
<compound-query> := <query> (<whitespace>? <compound-operator> <whitespace>? <query>)*

<whitespace> := (" " | \t)* \n? (" " | \t)*   // You cannot use more than one consecutive newline to split up queries.

<line-break> := (" " | \t)* \n (" " | \t)*

<compound-operator> := "|" | "&" | "~"

<omit-symbol> := "o"

<query> := <tag-start> | <node-start>

<tag-start> := <tag> (<omit-symbol>? <arrow-left> <omit-symbol>? <node-start>)?

<node-start> := <node> ((<omit-symbol>? <line-break>? "->" <line-break>? <omit-symbol>? <tag-start>) |                        
                        (<omit-symbol>? <line-break>? "->" <line-break>? <omit-symbol>? <node-start>) |
                        (<omit-symbol>? <line-break>? "<-" <line-break>? <omit-symbol>? <node-start>) |
                        (<omit-symbol>? <link> <omit-symbol>? <node-start>))?

<link> := <link-left> | <link-right>

<link-left> := <line-break>? "<-" <join> "-" <line-break>?

<link-right> := "-" <line-break>? <join> "->" <line-break>?

<join> := <left-join> | <inner-join> | <right-join>

<left-join> := "<" <inner-join>

<right-join> := <inner-join> ">"

<inner-join> := "{" <link-parameter-list> "}"

<node> := "[[" <node-title> <label-section>? <node-filter-section>? "]]"


```

...

### Peggy

...


[^1]: For simplicity and beauty I'm using the variant `:=` instead of `::=`. `*` means 0 or more occurrences. `+` means 1 or more occurrences. `?` means 0 or 1 occurrences. Parentheses are used for grouping. https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_form. 
