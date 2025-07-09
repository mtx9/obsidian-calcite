# Calcit - a graph query language for Obsidian

I created *Calcit* as a *graph query language* for [Obsidian](https://obsidian.md/) just for fun. It is inspired by *Cypher* from Neo4j. Using Cypher with Obsidan's markup syntax would have led to syntactic overhead. That's why I thought about a syntax to fit in seamlessly.

The basic approach is to "draw" the query like ASCII-art in a single line.
The result of the query is a graph that matches the definition of the textually defined graph.

*Example: Select all notes in the folder books and all notes that are linked to these files through the property `author`:*

```
[[books/*]]-{author}->[[*]]
```

---

### Basic elements

File nodes are addressed just like notes in Obsidian:
```
[[note]]
```

Links between notes:
```
->
<-
```

Links between notes defined by a property in a note:
```
{relation}
```

Tags can be addressed separately through:

```
(#tag)
```

Labels for notes can be addressed through:

```
[label]
```

The `:` precedes the filter section of the nodes:

```
[[note:property1, #tag1]]
```

### Examples

#### Notes and links

Select only the note `note1`:

```
[[note1]]
```

Select all notes that have the property `next`:

```
[[*:next]]
```

Select all notes that have the property next referencing another note, and the referenced notes.
(Notes that have the property next, but doesn't reference another note through it, aren't shown.):


```
[[*]]-{next}->[[*]]
```

Select all notes that have the property next and all notes that are referenced from these notes through next:

```
[[*:next]]-{next}->[[*]]
```

Select all notes that have either the property `friend` or `foe` (not both) and the notes referenced through these properties:

```
[[*]]-{friend ^| foe}->[[*]]
```

Select all notes frome the folder journal that have the tag `#music`:

```
[[journal/*:#music]]
```

Select all notes from the folder journal and all its recursive subfolders which have the tag `#music`.

`**/` stands for the folder and its recursive subfolders:

```
[[journal/**/*:#music]]
```

Select all notes from the folder journal which do not have the tag `#done`:

```
[[research/*:!#done]]
```

#### Links

Select all links from properties:

```
{*}
```

Select all links (also from the note's text bodies):
```
->
<-
```

Select the links from the properties `relation1` and `relation2` (e.g. for coloring them). (Writing `{relation1, relation2}` would select only pairs of these links between notes, since this is a logical AND):

```
{relation1} | {relation2}
```

Select all links between note1 and note2, without note1 and note2:

```
[[note1]]o->o[note2]
```

#### Tags

Select all tags:

```
(#*)
```

Select all tags that are in the notes in the folder books, without the notes in the folder books:
```
(#*)<-o[[books/*]]
```

Select `#tag1` and `#tag2`.

```
(#tag1) | (#tag2)
```

### Filtering

#### Filesystem wildcards

You can use the wildcards `*` and `?` like standard filesystem wildcards.

Select all notes:

```
[[*]]
```

Select notes starting with "2025-05-" to get all notes from May this year:

```
[[2025-05-??*]]
```

#### Properties and tags

`:` after the note's name defines the property and tags filter section.

`,` in the filter section or within a link block is an abbreviation for `&&` (logical and). 
You can use standard relational operators you know from C-like languages:

| Operator | Description |
| ---- | --- |
| `==` | EQUAL |
| `!=` | NOT EQUAL|
| `!`  | NOT |
| `&&` | AND |
| `\|\|` | OR  |
| `<` | LESS THAN |
| `>` | GREATER THAN |
| `<=` | LESS THAN OR EQUAL TO |
| `>=` | GREATER THAN OR EQUAL TO |
| `==` | EQUAL TO |


There is another one for XOR:

| Operator | Description |
| ---- | --- |
| `^\|` | XOR |

Select all nodes where `property1` equals 5 and `property2` is greater than 10 and have `#tag1` and do not have `#tag2`:
```
[[*:property1 == 5, property2 > 10, #tag1, !#tag2]]
```

#### Omitting nodes or tags

`o` at the connection spot of a link omits the nodes or tags.

Select all links from note1 to note2, without note1 and note2:

```
[[note1]]o->o[note2]
```

Select all tags that are in the notes in the folder books, without the notes in the folder books:
```
(#*)<-o[[books/*]]
```

#### Links

You can combine multiple property-links:

```
{relation1, relation2}

{relation1 && relation2}

{relation1 || relation2}

{relation1 ^| relation2}

{(relation1 ^| relation2) && (relation3 ^| relation4)}
```

### Joins

You can join notes through their properties similar as in SQL.

`|` after the node's title and before the filter section defines the alias section as usual.

#### Inner join

Select all notes from the folder actors and all notes from the folder writers that have the same `surname`:

```
[[actors/*|actor]]-{actor.surname == writer.surname}->[[writers/*|writer]]
```

#### Left join

`<` before the link's `{` means *left join*.

Select all notes from the folder actors and all notes from the folder writers that have the same `surname` and all notes from the folder actors (the left side).

```
[[actors/*|actor]]-<{actor.surname == writer.surname}->[[writers/*|writer]]
```

`[[*:next]]-{next}->[[*]]` could be expressed as:

```
[[*]]-<{next}->[[*]]
```

#### Right join

`>` after the link's `}` means *right join*.

Select all notes from the folder actors and all notes from the folder writers that have the same `surname` and all notes from the folder writers (the right side).

```
[[actors/*|actor]]-{actor.surname == writer.surname}>->[[writers/*|writer]]
```

### Compound queries

You can connect as many elements as you want to one query.

Select all actors who acted in the films and all directors who directed them:

```
[[actors/*]]-{acted_in}->[[films/*]]<-{directed}-[[directors/*]]
```

Additionally, you can combine graph queries. The operators (drawn from C bit operators) are:

| Operator | Description |
| --- | --- |
| `\|` | UNION | 
| `&` | INTERSECT |
| `~` | SUBTRACT |

Select all notes and substract all notes with the tag `#mathematics`.

```
[[*]] ~ [[*:#mathematics]]
```

You could also express this by negating the tag:

```
[[*:!#mathematics]]
```

In this case, both sides of the film notes are taken. But you can use the defined label `film`, which represents only the film nodes from the filtered query, to add other links. (This means, you only get the soundtrack for the films with actors and directors):
```
[[actors/*]]-{acted_in}->[[films/*|film]]<-{directed}-[[directors/*]] | [film]-{soundtrack}->[[music/*]]
```

## Implementation

For using Calcit in Obsidian, I plan to write the grammar for the [Peggy](https://peggyjs.org/) parser generator.
