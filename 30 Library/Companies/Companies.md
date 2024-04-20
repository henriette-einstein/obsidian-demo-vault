---
up: "[[30 Library]]"
tags:
  - Type/Tagpage
label: Companies
description: Notes about Companies
created: 2024-04-10
aliases:
  - "#Type/Company"
---
# [[30 Library]] > [[Companies]]

```dataview
list from !"90 System"
where econtains(file.etags, this.aliases[0])
sort file.name
```
