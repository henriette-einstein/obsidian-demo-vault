---
up: "[[30 Library]]"
tags:
  - Type/Tagpage
label: Posts
description: Medium Posts
created: 2024-04-10
aliases:
  - "#Type/Post"
---
# [[30 Library]] > [[Posts]]
```dataview
table published from !"90 System"
where econtains(file.etags, this.aliases[0])
sort file.name
```
