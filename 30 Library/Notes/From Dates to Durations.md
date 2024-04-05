---
tags:
  - Type/Note
created: 2024-03-14
categories:
  - "[[Medium Posts]]"
  - "[[Obsidian]]"
published: 2024-03-14
---
# From Dates to Durations: Perform Smart Time Calculations with Obsidian Dataview

The ability to track and calculate the duration between dates is a powerful tool for enriching our understanding and organization of information. The Obsidian Dataview plugin introduces a sophisticated yet intuitive way to handle this task. Whether you're calculating the duration of project timelines, the duration of historical events, or simply the age of a person, the Dataview plugin provides everything you need to perform the task. 

This article is a quick tip to get you started with duration calculations in Dataview.

To calculate a duration, you need a start-date and an end-date. With Dataview, you can simply subtract the start-date from the end-date to get the duration between the two points in time.

Consider a note on Winston Churchill that has a born and a died property in the frontmatter

```
---
tags: Person
born: 1874-11-30
died: 1965-01-24
---
# Winston S. Churchill

...
```

A Dataview query can easily calculate the age of Mr. Churchill:

```
table born, died, died - born as "age" from #Person
```

 If you only want to display the years and month of the age, Dataview provides a **durationformat** function, that handles that:

```
table born, died, durationformat(died - born,"y 'years' M 'month'") as "age" from #Person
```

Sometimes, you will encounter a note on a person who is still alive. Take Charles III. :

```
---
tags: Person
born: 1948-11-14
died:
---
# Charles III.

...
```

The above query would render a simple "-", what is usually not what you want. In most cases, you want the age of the person as the duration between today and this birthday. Nothing easier than that:

```
table born, date(today) - born as "age" from #Person
```

A potential usecase with a durationformat could be:

```
table born, durationformat(date(today) - born, "'was born ' y 'years and' M 'month ago'") as "born when" from #Person
```

This query displays the information, when - relative to today - a person was born.

In most cases, I need a query that displays the age of a person when he died, or - if the person is still alive - the current age of the person. The query only has to be adjusted slightly:

```
table born, died, durationformat(
choice(died, died, date(today)) - born, "y 'years and' M 'month'"
) as "age" 
from #Person
```

The query uses the **choice** function of Dataview. If there is a died-date, it uses that date. Otherwise, it uses the current date to perform the calculation. That works with any date properties you want to use for the calculation. The syntax is

```
durationformat(
   choice(<end property>, <end property>, date(today)) 
   - <start property>, 
   "y 'years' M 'month'"
)
```

