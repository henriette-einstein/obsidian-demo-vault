---
up: "[[30 Library]]"
tags:
  - Type/Tagpage
label: Organizations
description: Notes about Organizations
created: 2024-04-12
aliases:
  - "#Type/Organization"
---
# [[30 Library]] > [[Organizations]]
## Notes
```dataview
list from !"90 System"
where econtains(file.etags, this.aliases[0])
sort file.name
```
