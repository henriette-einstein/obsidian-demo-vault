---
up: "[[30 Library]]"
tags:
  - Type/Tagpage
label: Books
description: Notes on books
created: 2024-03-06
aliases:
  - "#Type/Book"
cssclasses:
  - banner-image
banner: "[[Books.webp]]"
---
> [!banner-image] ![[Books.webp|py-70]]
# [[40 Sources|Sources]] > [[Books|Books]]
```dataview
table authors, categories from !"90 System"
where econtains(file.etags, this.aliases[0])
sort file.name
```
