---
up: "[[30 Library]]"
tags:
  - Type/Tagpage
label: "{{title}}"
description: 
created: "{{date}}"
aliases:
---
# [[30 Library|Library]] > [[{{title}}]]

```dataview
list from !"90 System"
where econtains(file.etags, this.aliases[0])
sort file.name
```
