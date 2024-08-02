---
description: 
created: 2024-04-09
type: "[[MOCs]]"
---
# [[C - History MOCs]]

```dataview
table mocs as "Topics" from !"90 System" and !outgoing([[]])
where type = this.type and contains(collections, [[]])
sort file.name
```
