---
type: "[[Queries]]"
created: 2024-03-07
label: Recently imported highlights
description: Highlights imported from Readwise the last 5 days
---
# [[Recent Highlights|Recently imported Highlights]]

```dataview
table "[[" + dateformat(file.cday,"yyyy-MM-dd") + "]]" as "Created" from !"90 System" 
where type = [[Book Highlights]] or type = [[Article Highlights]]
and (file.ctime >= date(today) - dur(5 day))
sort file.ctime
```
# See also
- [[Recent Notes|Recently created notes]]
- [[Notes without MOCs and Up Reference|Uncategorized Notes]]
- [[Untyped Notes|Untyped Notes]]