## Annotations and special use cases

Select all links from note1 to note2, without note1 and note2 and without all links through properties:

```
[[note1]]o-{!*}->o[note2]
```

Select note1 and all notes that are linked from note1, but omit the links:

```
[[note1]]-o->[[*]]
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

If you want to display the extended graph view inside of a note by using the query language, it could look like this:
````
```calcite
query: '[[actors/*]]-{acted_in}->[[films/*|film]]<-{directed}-[[directors/*]] | [film]-{soundtrack}->[[music/*]]'

size: medium
linkTypes:
   global: true
   colorPalette: 'cubehelix'
```
````
