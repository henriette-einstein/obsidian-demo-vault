---
type: "[[Queries]]"
created: 2024-03-07
aliases:
  - Untyped Notes
label: Untyped Notes
description: Notes with no tags attached
---
# [[Untyped Notes]]

```dataview
table "[[" + dateformat(file.cday,"yyyy-MM-dd") + "]]" as "Created" from !"90 System" 
where !type and !up
sort file.ctime
```

# See also
- [[Notes without MOCs and Up Reference|Uncategorized Notes]]
- [[Recent Notes|Recently created notes]]
- [[Recent Highlights|Recently imported Highlights]]
