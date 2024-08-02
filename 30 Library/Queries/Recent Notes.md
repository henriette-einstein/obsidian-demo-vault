---
label: Recently created notes
description: New notes added the last 5 days
created: 2024-03-07
aliases:
  - Recently created notes
type: "[[Queries]]"
---
# [[Recent Notes|Recently created notes]]

```dataview
table "[[" + dateformat(file.cday,"yyyy-MM-dd") + "]]" as "Created" from !"90 System" WHERE (file.ctime >= date(today) - dur(5 day))
sort file.ctime desc
limit 30
```

# See also
- [[Recent Highlights|Recently imported Highlights]]
- [[Notes without MOCs and Up Reference]]
- [[Untyped Notes|Untyped Notes]]