## Annotations and special use cases

Select all links from note1 to note2, without note1 and note2 and without all links through properties:

```
[[note1]]o-{!*}->o[note2]
```

If you have properties with similar names (`author1`, `author2`, `author3`, ...), you can adress them with wildcards:

```
[[books/*]]-{author*}->[[*]]
```

### Metadata of notes

You can use `°` to access the metadata of the notes. This selects all notes that are at least 1 KB in size:

```
[[*:°size >= 1024]]
```

Select all notes that are equal in size:

```
[[*|note1]]-{note1°size == note2°size}->[[*|note2]]
```


For convenience the names of the metadata variables are the same as under the field *file* in the dataview plugin:

https://blacksmithgu.github.io/obsidian-dataview/annotation/metadata-pages/

### Inline notation

If you want to use Calcite inside of a note with compound operators, you need to omit the whitespace beside the compound operators.

```
[[actors/*]]-{acted_in}->[[films/*|film]]<-{directed}-[[directors/*]]|[film]-{soundtrack}->[[music/*]]
```

Or you could write it in a codeblock:
````
```calcite
[[actors/*]]-{acted_in}->[[films/*|film]]<-{directed}-[[directors/*]] | [film]-{soundtrack}->[[music/*]]
```
````
