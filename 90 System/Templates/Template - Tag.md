---
type: "[[Tags]]"
created: "{{date}}"
mocs: 
aliases:
---
```dataview
table type, mocs from !"90 System" and !outgoing([[]])
where econtains(file.etags, this.aliases[0])
sort type, file.name
```
