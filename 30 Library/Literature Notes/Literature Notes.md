---
up: "[[30 Library]]"
tags:
  - Type/Tagpage
label: Literature Notes
description: Literature Notes
created: 2024-04-13
aliases:
  - "#Type/Litnote"
---
# [[30 Library]] > [[Literature Notes]]
## Notes
```dataview
table up as "in" from !"90 System"
where econtains(file.etags, this.aliases[0])
sort file.name
```
