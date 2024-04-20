---
up: "[[30 Library]]"
tags:
  - Type/Tagpage
label: Countries
description: Notes about Countries
created: 2024-04-09
aliases:
  - "#Type/Country"
cssclasses:
  - cards
---
# [[30 Library|Library]] > [[Countries]]
## Notes
```dataview
table "![flag|400]("+flag+")", capital from !"90 System"
where econtains(file.etags, this.aliases[0])
sort file.name
```
