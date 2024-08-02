---
type: 
created: "{{date}}"
mocs:
---
# [[{{title}}]]

```dataview
table mocs from !"90 System" and !outgoing([[]])
where type = this.type and contains(collections, [[]])
sort file.name
```

