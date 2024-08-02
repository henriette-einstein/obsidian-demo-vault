---
created: 2024-08-02
type: "[[Locations]]"
---
# [[C - Refineries]]

```dataview
table country, owners, capacity from !"90 System" and !outgoing([[]])
where type = this.type and contains(collections, [[]])
sort file.name
```
