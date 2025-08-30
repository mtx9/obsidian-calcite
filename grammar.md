
## Grammar

### Description

The first part of this file states *Calcite* in *Backus–Naur Form*[^1]. The second part describes it as grammar for the [Peggy](https://peggyjs.org/) parser generator.

### Definition

```
<compound-query> := (<query> | <link> | <tag>) (<whitespace>? <compound-operator> <whitespace>? (<query> | <link> | <tag>))*


<whitespace> := (" " | \t)* \n? (" " | \t)*   

<line-break> := (" " | \t)* \n (" " | \t)*

<compound-operator> := "|" | "&" | "~"

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

<inner-join> := "{" <link-parameter-list> "}"

<link-parameter-list> := <link-property> (<white-space>? "," <white-space>? <link-property>)*

<link-parameter-expression> := <unary-link-parameter-expression> |
                               (<link-parameter-expression> <white-space>? <link-parameter-operator> <white-space>? <link-parameter-expression>) |
                               ( "(" <white-space>? <link-parameter-expression> <white-space>? <link-parameter-operator> <white-space>? <link-parameter-expression> <white-space>? ")")

<unary-link-parameter-expression> := <link-property> | <number> | <string> | <date>

<link-parameter-operator> := <logical-operator> | <relational-operator>

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

<filter-parameter> := <filter-parameter-expression> | <tag-expression>

<filter-parameter-expression> := <unary-filter-parameter-expression> |
                                (<filter-parameter-expression> <whitespace>? <filter-parameter-operator>
                                   <whitespace>? <filter-parameter-expression>) |
                                ("(" <whitespace>? <filter-parameter-expression> <whitespace>? <filter-parameter-operator>
                                   <whitespace>? <filter-parameter-expression> <whitespace>? ")") 

<unary-filter-parameter-expression> := <filter-property> | <number> | <string> | <date>

<filter-parameter-operator> := <logical-operatory> | <relational-operator>

<filter-property> := "!"? "°"? <property-name>

<property-name> := [a-zA-Z0-9_\-.*?\\]+


<tag> := "(" (<whitespace>? <tag-expression> (<whitespace>? "," <whitespace>? <tag-expression>)* ")"

<tag-expression> := <tag-name> | (<tag-expression> <whitespace>? <logical-operator> <whitespace>? <tag-expression>) |
                                 ("(" <whitespace>?  <tag-expression> <whitespace>? <logical-operator> <whitespace>? <tag-expression> <whitespace>? ")")

<tag-name> := "!"? "#" <tag-character>* <alphanumeric-character> <tag-character>*

<tag-character> := [a-zA-Z0-9_\-/*?]

<alphanumeric-character> := [a-zA-Z0-9]

<date> := "`" ((<year>? ("-" <month>? ("-" <day>?)?)?) | (<year>? ("/" <month>? ("/" <day>?)?)?))  "`"

<year> := {<digit>}4

<month> := {<digit>}2

<day> := {<digit>}2

<number> := "-"? [0-9]+ ("." [0-9]+)?

<string> := "\"" /*An arbitrary number of any character and quoted double apostrophe. */ "\""


```

#### Annotations

You cannot use more than one consecutive newline to split up queries.

You can omit a property-based link by writing `o` before or after the link. But I recommend writing `o` before and after the link, as this is more beautiful (symmetrical).

```
[[books/*]]-o{author}o->[[authors/*]]
```

##### Dates

Dates are written in the form `` `YYYY-MM-DD` `` or `` `YYYY/MM/DD` `` (with backticks). If you only want to compare a part of the date, you can omit the other date numbers but you have to keep the delimiters. Though, you can omit parts of the date from right to left and even omit the delimiters.

Select all notes from the folder trips whose date `start` is equal or greater than `2020-02-14`:

```
[[trips/*:start >= `2020-02-14`]]

[[trips/*:start >= `2020/02/14`]]
```
Select all notes from the folder trips  whose date `start` is greater than the year 2020 and greather than the 2nd of a month. (This means the first two days of any month are not selected.):

```
[[trips/*:start > `2020--02`]]

[[trips/*:start > `2020//02`]]
```

Select all notes from the folder trips whose date `start`is  greater than May 2023:

```
[[trips/*:start > `2023-05`]]

[[trips/*:start > `2023/05`]]
```

Select all notes from the folder trips whose date `start` is equal or greater than `2025`:

```
[[trips/*:start >= `2025`]]
```

Select all notes from the folder trips whose date `start` is greater than the 16th day of the month:

```
[[trips/*:start > `--16`]]

[[trips/*:start > `//16`]]
```

##### Tags

> Tags must contain at least one non-numerical character. For example, #1984 isn't a valid tag, but #y1984 is.
> https://help.obsidian.md/tags#Tag+format

`*` and `?` are allowed within a property name by Obsidian. To filter nodes with the properties named `*` or `?` you have to quote them with `\`:

```
[[*:\?]] | [[*:\*]]
```

### Peggy

...


[^1]: For simplicity and beauty I'm using the variant `:=` instead of `::=`. `*` means 0 or more occurrences. `+` means 1 or more occurrences. `?` means 0 or 1 occurrence. `()` are used for grouping. `[]` are used to define a collection of characters, whereby `-` defines a range and `\` quotes characters. A number succeeding a `}` means a certain number of repetitions. https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_form.
