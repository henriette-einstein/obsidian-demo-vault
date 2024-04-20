---
up: "[[30 Library]]"
tags:
  - Type/Tagpage
label: Locations
description: Notes about Locations
created: 2024-04-11
aliases:
  - "#Type/Location"
---
# [[30 Library]] > [[Locations]]
## Notes
```dataview
table "["+file.name+"](geo:"+location+")" as "Location on Map" from !"90 System"
where econtains(file.etags, this.aliases[0])
sort file.name
```
