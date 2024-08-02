---
type: "[[Queries]]"
created: 2024-03-07
aliases:
  - Uncategorized Notes
label: Uncategorized Notes
description: Notes not attached to a category
---
# [[Notes without MOCs and Up Reference]]

```dataview
table "[[" + dateformat(file.cday,"yyyy-MM-dd") + "]]" as "Created" from !"90 System" 
where !mocs and !up
sort file.ctime
```

# See also
- [[Untyped Notes|Untyped Notes]]
- [[Recent Notes|Recently created notes]]
- [[Recent Highlights|Recently imported Highlights]]

