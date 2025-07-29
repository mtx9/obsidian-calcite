## Grammar

### Description

The first part of this file states *Calcit* in *Backus–Naur Form*[^1]. The second part describes it as grammar for the [Peggy](https://peggyjs.org/) parser generator.

### Definition

```
<compound-query> := <query> (<whitespace>? <compound-operator> <whitespace>? <query>)*

<whitespace> := (" " | \t)* \n? (" " | \t)*   

<line-break> := (" " | \t)* \n (" " | \t)*

<compound-operator> := "|" | "&" | "~"

<query> := <tag-start> | <node-start>

<tag-start> := <tag> ("o"? <link-arrow-left> "o"? <node-start>)?

<node-start> := <node> (("o"? <line-break>? <link-arrow-right> <line-break>? "o"? <tag-start>) |                        
                        ("o"? <line-break>? <link-arrow-right> <line-break>? "o"? <node-start>) |
                        ("o"? <line-break>? <link-arrow-left> <line-break>? "o"? <node-start>) |
                        ("o"? <link> "o"? <node-start>))?

<link-arrow-right> := "->" | "-o->"

<link-arrow-left> := "<-" | "<-o-"

<link> := <link-left> | <link-right>

<link-left> := <line-break>? "<-" "o"? <join> "o"? "-" <line-break>?

<link-right> := "-" <line-break>? "o"? <join> "o"? "->" <line-break>?

<join> := <left-join> | <inner-join> | <right-join>

<left-join> := "<" <inner-join>

<right-join> := <inner-join> ">"

<inner-join> := "{" <link-parameter-list> "}"

<node> := "[[" <node-title> <label-section>? <filter-section>? "]]"

<label-section> := "|" <whitespace>? <label-name>

<filter-section> := ":" <whitespace>? <filter-parameter-list>

<label> := "[" <label-name> "]"

<tag> := "(" (<whitespace>? <tag-field> (<whitespace>? ("," (<whitespace>? <tag-field> (<whitespace>?)* ")"

<tag-field> := <tag-name> |
               (<tag-name> <whitespace>? <tag-operator> <whitespace>? <tag-name>) |
               (<tag-name> <whitespace>? <tag-operator> <whitespace>? <tag-field-with-parentheses>) |
               (<tag-field-with-parentheses> <whitespace>? <tag-operator> <whitespace>? <tag-name>) |
               (<tag-field-with-parentheses> <whitespace>? <tag-operator> <whitespace>? <tag-field-with-parantheses>)

<tag-field-with-parantheses> := "(" ((<tag-name> <whitespace>? <tag-operator> <whitespace>? <tag-name>) |
                                     (<tag-name> <whitespace>? <tag-operator> <whitespace>? <tag-field-with-parantheses>) |
                                     (<tag-field-with-parantheses> <whitespace>? <tag-operator> <whitespace>? <tag-name>) |
                                     (<tag-field-with-parantheses> <whitespace>? <tag-operator> <whitespace>? <tag-field-with-parantheses>)) ")"



                             

```

...

#### Annotations

You cannot use more than one consecutive newline to split up queries.

You can omit a property-based link by writing `o` before or after the link. But I recommend writing `o` before and after the link, as this is more beautiful (symmetrical).

```
[[books/*]]-o{author}o->[[authors/*]]
```

### Peggy

...


[^1]: For simplicity and beauty I'm using the variant `:=` instead of `::=`. `*` means 0 or more occurrences. `+` means 1 or more occurrences. `?` means 0 or 1 occurrences. Parentheses are used for grouping. https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_form. 
