## Annotations and special use cases

Select all links from note1 to note2, without note1 and note2 and without all links through properties:

```
[[note1]]o-{!*}->o[note2]
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
