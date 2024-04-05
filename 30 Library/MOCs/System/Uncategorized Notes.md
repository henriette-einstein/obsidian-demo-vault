---
tags:
  - system
  - Type/MOC
created: 2024-03-07
aliases:
  - Uncategorized Notes
label: Uncategorized Notes
description: Notes not attached to a category
---
# [[Uncategorized Notes]]

```dataview
table "[[" + dateformat(file.cday,"yyyy-MM-dd") + "]]" as "Created" from !"90 System" and !#Type/Category and !#system and !#Type/Tagpage and !#Type/Section and !#Type/Cal and !#Type/Gallery where !categories
sort file.ctime
```

# See also
- [[Untyped Notes|Untyped Notes]]
- [[Recent Notes|Recently created notes]]
- [[Recent Highlights|Recently imported Highlights]]

