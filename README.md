# Calcit - A graph query language for Obsidian

I created *Calcit* as a *graph query language* for [Obsidian](https://obsidian.md/) just for fun. It is inspired by *Cypher* from Neo4j. Using Cypher by integrating Obsidan's markup syntax would have led to syntactic overhead. That's why I thought about a syntax to fit in seamlessly.

The basic approach is to "draw" the query like ASCII-art in a single line.
The result of the query is a graph that matches the definition of the textually defined graph.

```
[[*]]-{next}->[[*]]
```

---

### Basic elements

Nodes are addressed just like notes in Obsidian:
```
[[note]]
```

Links between notes:
```
->
```

Links between notes defined by a property in a note:
```
{relation}
```

The `:` precedes the filter section of the nodes:

```
[[note:property1, #tag1]]
```

### Examples

Select only the note `note1`:

```
[[note1]]
```

Select all notes that have the property `next`:

```
[[*:next]]
```

Select all notes that have the property next referencing another note, and the referenced notes.
(Notes that have the property next, but doesn't reference another note, aren't shown.):


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

Select all notes frome the folder journal which have the tag #music:

```
[[journal/*:#music]]
```

Select all notes from the folder journal and all its recursive subfolders which have the tag #music.

`**/` stands for the folder and its recursive subfolders:

```
[[journal/**/*:#music]]
```

Select all notes from the folder journal which have not the tag #done:

```
[[research/*:!#done]]
```

### Filtering

#### Filesystem wildcards

You can use the wildcards `*` and `?` like standard filesystem wildcards.

Select all notes:

```
[[*]]
```

Select notes starting with "2025-05-" to get all notes from may this year:

```
[[2025-05-??*]]
```

#### Properties and tags

`:` after the note's name defines the property and tags filter section.

`,` in the filter section or within the relation block is an abbreviation for `&&` (logical and). 
You can use standard relational operators you know from C-like languages:

| Operator | Description |
| ---- | --- |
| `==` | EQUAL |
| `!`  | NOT |
| `&&` | AND |
| `\|\|` | OR  |

(...add the rest)

There is another one for XOR:

| Operator | Description |
| ---- | --- |
| `^\|` | XOR |

```
[[note:property1 == 5, property2 > 10]]
```

### Relations

Examples:

```
{relation1, relation2}

{relation1 && relation2}

{relation1 || relation2}

{relation1 ^| relation2}
```

More complex examples are:

{relation1 && (relation2 ^| relation3)}
