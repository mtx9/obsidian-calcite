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

<tag-start> := <tag> (<omit-symbol>? "<-" <omit-symbol>? <node-start>)?

<node-start> := <node> ((<omit-symbol>? <line-break>? "->" <line-break>? <omit-symbol>? <tag-start>) |                        
                        (<omit-symbol>? <line-break>? "->" <line-break>? <omit-symbol>? <node-start>) |
                        (<omit-symbol>? <line-break>? "<-" <line-break>? <omit-symbol>? <node-start>) |
                        (<omit-symbol>? <link> <omit-symbol>? <node-start>))?

<link> := <link-left> | <link-right>

<link-left> := <line-break>? "<-" <omit-symbol>? <join> <omit-symbol>? "-" <line-break>?

<link-right> := "-" <line-break>? <omit-symbol>? <join> <omit-symbol>? "->" <line-break>?

<join> := <left-join> | <inner-join> | <right-join>

<left-join> := "<" <inner-join>

<right-join> := <inner-join> ">"

<inner-join> := "{" <link-parameter-list> "}"

<node> := "[[" <node-title> <node-label-section>? <node-filter-section>? "]]"

<node-label-section> := "|" <whitespace>? <label-name>

<node-filter-section> := ":" <whitespace>? <node-filter-parameter-list>

```

...

#### Annotations

You can omit a property-based link by writing `o` before or after the link. But I recommend writing `o` before and after the link, as this is more beautiful (symmetrical).

´´´
[[books/*]]-o{author}o->[[authors/*]]
´´´

### Peggy

...


[^1]: For simplicity and beauty I'm using the variant `:=` instead of `::=`. `*` means 0 or more occurrences. `+` means 1 or more occurrences. `?` means 0 or 1 occurrences. Parentheses are used for grouping. https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_form. 
