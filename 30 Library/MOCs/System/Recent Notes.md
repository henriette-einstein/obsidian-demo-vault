---
tags:
  - system
  - Type/MOC
label: Recently created notes
description: New notes added the last 5 days
created: 2024-03-07
aliases:
  - Recently created notes
---
# [[Recent Notes|Recently created notes]]

```dataview
table "[[" + dateformat(file.cday,"yyyy-MM-dd") + "]]" as "Created" from !"90 System" WHERE (file.ctime >= date(today) - dur(5 day))
sort file.ctime
limit 30
```

# See also
- [[Recent Highlights|Recently imported Highlights]]
- [[Uncategorized Notes]]
- [[Untyped Notes|Untyped Notes]]