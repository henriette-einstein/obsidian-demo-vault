---
type: "[[Tags]]"
created: 2024-08-02
mocs: 
aliases:
  - "#pinned"
---
```dataview
table type, mocs from !"90 System" and !outgoing([[]])
where econtains(file.etags, this.aliases[0])
sort type, file.name
```
