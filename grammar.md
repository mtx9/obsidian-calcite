## Grammar

### Description

The first part of this file states *Calcite* in *Backus–Naur Form*[^1]. The second part describes it as grammar for the [Peggy](https://peggyjs.org/) parser generator.

### Definition

```
<compound-query> := (<query> | <link> | <tag>) (<whitespace>? <compound-operator> <whitespace>? (<query> | <link> | <tag>))*


<whitespace> := (" " | \t)* \n? (" " | \t)*   

<line-break> := (" " | \t)* \n (" " | \t)*

<compound-operator> := "|" | "&" | "~"

<arithmetic-operator> := "+" | "-" | "*" | "/" | "%"

<logical-operator> := "&&" | "||" | "^|"

<relational-operator> := "<" | ">" | "<=" | ">=" | "==" | "!="


<query> := <tag-start> | <node-start>


<tag-start> := <tag> ("o"? <link-arrow-left> "o"? <node-start>)?


<node-start> := <node> (("o"? <line-break>? <link-arrow-right> <line-break>? "o"? <tag-start>) |                        
                        ("o"? <line-break>? <link-arrow-right> <line-break>? "o"? <node-start>) |
                        ("o"? <line-break>? <link-arrow-left> <line-break>? "o"? <node-start>) |
                        ("o"? <link-embedded> "o"? <node-start>))?


<link-arrow-right> := "->" | "-o->"

<link-arrow-left> := "<-" | "<-o-"


<link-embedded> := <link-left> | <link-right>

<link-left> := <line-break>? "<-" "o"? <join> "o"? "-" <line-break>?

<link-right> := "-" <line-break>? "o"? <join> "o"? "->" <line-break>?

<join> := <left-join> | <inner-join> | <right-join>

<left-join> := "<" <inner-join>

<right-join> := <inner-join> ">"

<inner-join> := "{" <link-property-list> "}"

<link-property-list> := <link-property> (<white-space>? "," <white-space>? <link-property>)*

<link-property> = "!"? (<label-name> ("." | "°"))? <property-name>

<link> := <inner-join>


<node> := <label> | ("[[" <node-section> <label-section>? <filter-section>? "]]")

<node-section> := (<node-path> "/")? <node-title>

<node-path> := (<path-character-without-slash>+ "/")* <path-character-without-slash>+

<path-character-without-slash> := [a-zA-Z0-9.,_\-*?]

<node-title> := [a-zA-Z0-9.,_\-/*?]+

<label-section> := "|" <label-name>

<label-name> := [a-zA-Z0-9.,_\-]+


<label> := "[" <label-name> "]"


<filter-section> := ":" <whitespace>? <filter-parameter-list>

<filter-parameter-list> := <filter-parameter> (<white-space>? "," <white-space>? <filter-parameter>)*

<filter-parameter> := <filter-property-expression> | <tag-expression>

<filter-property-expression> := <filter-property> |
                                (<filter-property-expression> <whitespace>? <filter-property-operator> <whitespace>? <filter-property-expression>) |
                                ("(" <whitespace>? <filter-property-expression> <whitespace>? <filter-property-operator> <whitespace>? <filter-property-expression> <whitespace>? ")") 

<filter-property> := "!"? "°"? <property-name>

<property-name> := [a-zA-Z0-9_\-.*?\\]+

<filter-property-operator> := <logical-operatory> | <arithmetic-operator>

<tag> := "(" (<whitespace>? <tag-field> (<whitespace>? "," <whitespace>? <tag-field>)* ")"

<tag-name> := "!"? "#" <tag-character>* <alphanumeric-character> <tag-character>*

<tag-character> := [a-zA-Z0-9_\-/*?]                          

/* Add the expressions for the <link-property-list>, rename it to <link-parameter-list>.
   Make it clear when arithmetic operaters are allowed within the <link-property-list> and <filter-parameter-list>.
   Add the expressinos for the <tag-parameter-list>. */

```

...

#### Annotations

You cannot use more than one consecutive newline to split up queries.

You can omit a property-based link by writing `o` before or after the link. But I recommend writing `o` before and after the link, as this is more beautiful (symmetrical).

```
[[books/*]]-o{author}o->[[authors/*]]
```

> Tags must contain at least one non-numerical character. For example, #1984 isn't a valid tag, but #y1984 is.
> https://help.obsidian.md/tags#Tag+format

`*` and `?` are allowed within a property name by Obsidian. To filter nodes with the properties named `*` or `?` you have to quote them with `\`:

```
[[*:\?]] | [[*:\*]]
```

### Peggy

...


[^1]: For simplicity and beauty I'm using the variant `:=` instead of `::=`. `*` means 0 or more occurrences. `+` means 1 or more occurrences. `?` means 0 or 1 occurrence. `()` are used for grouping. `[]` are used to define a collection of characters, wherby `-` defines a range and `\` quotes characters. https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_form.
