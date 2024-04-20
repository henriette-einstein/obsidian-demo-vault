---
up: "[[30 Library]]"
tags:
  - Type/Tagpage
label: People
description: Notes about People
created: 2024-04-09
aliases:
  - "#Type/Person"
---
# [[30 Library]] > [[People]]
## Notes
```dataview
table country, born, died from !"90 System"
where econtains(file.etags, this.aliases[0])
sort file.name
```
