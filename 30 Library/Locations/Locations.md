---
up: "[[30 Library]]"
label: Locations
description: Notes about Locations
created: 2024-04-11
---
# [[30 Library|Library]] > [[Locations]]

## Collections
- [[C - Oilfields]]
- [[C - Refineries]]
## Notes
```dataview
table collections, "["+file.name+"](geo:"+location+")" as "Location on Map" 
from !"90 System" and !outgoing([[]])
where type = [[]]
sort file.name
```
