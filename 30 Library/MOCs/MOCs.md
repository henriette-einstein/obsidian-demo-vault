---
up: "[[30 Library]]"
label: MOCs
description: Maps of Content and Index Pages
created: 2024-03-05
cssclasses:
  - banner-image
banner: "[[MOCs.webp]]"
---
> [!banner-image] ![[MOCs.webp]]
#  [[MOCs]]

## Collections
- [[C - History MOCs]]
- [[C - Middle East MOCs]]
- [[C - Oil MOCs]]
## Notes
```dataview
table mocs,description from !"90 System" and !outgoing([[]])
where type = [[]] and !collections
sort up,file.name
```
