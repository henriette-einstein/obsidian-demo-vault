---
tags:
  - Type/Post
  - pinned
created: 2024-04-17
categories:
  - "[[Medium Posts]]"
  - "[[Obsidian]]"
types:
  - "[[Posts]]"
published: 2024-04-19
---
# [[Adding Interactive Maps to Your Obsidian Vault]]

![[Map of Middle East on Wall.jpeg]]

In today's news, the Middle East often gets a lot of attention. The history of the modern Middle East is important for understanding its economic and political situation, as well as the reasons for its ongoing conflicts. That is why I am starting a one-year personal project, “**The History of the Modern Middle East in 12 Months**”, to gain a more profound understanding of the current situation. 

In the project's initial month, I plan to explore the geography of the region. My goal is to grasp the essentials about the Middle Eastern countries, their borders, and the significant places that have had an impact on both historical and modern events. To enhance this learning experience, I will incorporate spatial information into an interactive map. Clicking on different countries and key locations will help to discover in-depth information about each one. Here is a preview of what I intend to build:

![[Map of Middle East.png]]

Using a little help from AI, I was able to create initial notes and maps for all countries of the Middle East in one afternoon. How that can be done is described in this article.
## Prerequisites

I am using Obsidian as my PKM system. Obsidian has numerous plugins that can be used to add additional functionality. After a quick research, I found two plugins that can be used to add interactive maps to Obsidian:

- [Leaflet](https://github.com/javalent/obsidian-leaflet) by Jeremy Valentine
- [Map View](https://github.com/esm7/obsidian-map-view) by esm

Leaflet appears to have more functionality, but Map View has everything I need and looks much simpler to use. Therefore, I chose Map View as the plugin for my project. The plugin can be installed in the Obsidian settings under **Community Plugins**.

In addition to that, I use the [Obsidian Modular CSS Layout](https://efemkay.github.io/obsidian-modular-css-layout/) by [Faiz Khuzaimah](https://github.com/efemkay). I merely use the Float Callout functionality to place a country's flag to the right side of the basic information in the countries Note, as seen in the sample below.

---
![[Saudi Arabia 1.png|500]]

---

## Adding Spatial Information to a Note

The Map View plugin supports two mechanisms to add spatial information to a note

1. Add the geolocation in the YAML frontmatter of the note
2. Add one or more inline geolocations to a note

Both mechanisms can be triggered via the Obsidian command palette as shown below

![[Map view in command Palette.png|500]]

When you invoke that command, you'll be prompted to fill in the location you're looking for. After selecting the requested result, the geolocation will be inserted in the frontmatter or as an inline link. All geolocations of the note are then visible in the Map View, as shown in the introduction to this article. By clicking on the marker in Map View, one can jump to the appropriate note.

By default, the Map View displays all geolocations in the vault. But it is possible to only show those locations that match a filter criterion.

![[Map View Filters.png]]

The most important filters are

- **path**: that displays only those locations that are used in a note with the given path
- **linkedto**: that displays the locations that are used in all notes that have a link to the specified note
- **linkedfrom**: that displays the locations that are used in all notes that the specified note links to

Here's a simple example of how to use these filters: I have a note called "Oil Producing Countries in the Middle East" that links to all the countries that produce oil in the region. The filter

```
linkedfrom:"30 Library/Zettel/Oil Producing Countries in the Middle East.md" 
```

then displays only those countries on the map.

![[Map View Oil Producing.png]]
## Add individual Marker Icons

Without any further customization, the Map View displays the locations with a simple blue marker with a white circle in the middle. But it is possible to configure other icons for locations with specific tags in the plugins settings page.

The following screenshot shows my configuration for geolocations with the tags Type/City, Type/Country, Type/Location and Type/Event. 

![[Map View Custom Icons.png]]

If you use geolocations in the YAML frontmatter of a note, the tag-information is taken from the tags-property of the note. When using inline locations, you have to append the tag after the locations link.
## Adding a Map to a Note

The Map View plugin also allows adding maps to a note. The embedded maps can use filters and appropriate zoom levels. 

Here is an example of the map of Saudi Arabia placed on the bottom of the note about that country. I just set the filter "path" to the path of the Saudi Arabia note.

![[Saudi Arabia 2.png]]
Other, more complex embedded maps, can easily be constructed using the "linkedto" filter. Examples may be
- Kingdoms in the Middle East
- Sunnite Countries in the Middle East
- etc.

## Using Templates for embedded Maps

A map embedded into a note is just a specially marked code block in the notes body text. The following screenshot shows the code block of the map embedded in the note about Saudi Arabia.

![[Map View Codeblock 1.png]]

Instead of "hard-coding" the notes path, it is also possible to use the variable **\$filename\$** in the code block. In that case, the marker(s) of the currently open note are displayed in the embedded map.

![[Map View Codeblock 2.png]]

This variable can be used to create an Obsidian template for inserting a map via the standard "Insert template" command. However, the geolocations used to center the map do not support variables and have to be hard-coded in the template. I therefore use the geolocation of the city I live in (Würzburg in Germany) in my templates.

![[My Map View Template.png]]

In total, I created four templates that are equal except for the line containing the query.

The "Map of Current Note Name Template" uses
```
"query":"name:\"$filename$\"",
```

The "Map of Current Note Path Template" uses
```
"query":"path:\"$filename$\"",
```

The "Map of Notes linked from Current Note Template" uses
```
"query":"linkedfrom:\"$filename$\"",
```

The "Map of Notes linked to Current Note Template" uses
```
"query":"linkedto:\"$filename$\"",
```

My workflow for creating notes about the countries in the Middle East therefore is quite straightforward:

1. Create a note for each country
2. As usual, add information about that country
3. Add the geolocation of the country with the appropriate Map View command
4. Insert the template "Map of Current Note Path Template" to add an embedded map to the note
5. Adjust the center and the zoom level of the map

## Use a Little Help from AI 

The workflow sketched above still required quite a lot of effort to set up an initial note for a single country. Since I wanted to use a similar structure for all my country notes, I created a simple prompt asking ChatGPT (I am using the paid version ChatGPT4) to collect the required information and return it in Markdown format.

If you want to try it too, just copy the source below and replace "Saudi Arabia" with the country you are interested in. Just copy the result and paste it into an empty Obsidian note, and you are ready to go.

```
You are an experienced historian. I am a history enthusiast who wants to study the influence of the Oil industry on the modern history of the middle east. 

Give me a Markdown note for the country Saudi Arabia using the following template:


"
---
location: <Latitude coordinate of the country as number only>,<Longitude coordinate of the country as number only>
---

# Country Profile: [Country Name]

<Provide a short overview of the country.>

## Overview

> [!blank|float-right-small]
> ![Flag](<Link to the raw image of the flag of the country on Wikipedia>)

- **Capital**: <Capital City>
- **Population**: <Population>
- **Area**: <Area in square kilometers>
- **Major Cities**: <List major cities>
- **Official Language(s)**: <Language(s)>
- **Currency**: <Currency>


## Historical Context

<Provide a detailed overview of the country's history, focusing on its formation and major historical events that have shaped its current political and economic landscape before the modern era.>

### Modern Era

<Provide a detailed overview of the country's history, focusing on its formation and major historical events that have shaped its current political and economic landscape in the modern era.>

## Important Events
<Important Events that are related to the city. Give at least 5 events and 10 events as a maximum>

## Important People
<Important People that lived in the city. Focus on politicians, religious leaders, soldiers and entrepreneurs>


## Oil Industry Overview

### History of Oil Discovery and Development

- **First Discovery**: <Year/Location>
- **Major Oil Fields**: <List of major oil fields>
- **Production Statistics**:
  - **Oil Production**: <Barrels per day, as of last recorded year>
  - **Gas Production**: <Cubic meters per day, as of last recorded year>
- **Impact on Economy**: <Discuss how oil has impacted the economy>

### Key Players

- **National Oil Companies**: <List of companies>
- **International Oil Companies in the Region**: <List of companies>

## Geopolitical Influence

- **Regional Politics**: How does the oil industry affect its regional political stance?
- **International Relationships**: How have oil resources influenced international relations?

## Current Challenges

Discuss current issues facing the country related to the oil industry, such as economic dependency, environmental concerns, political instability, or other relevant issues.

## Maps and Diagrams

```mapview
{"name":"Default",
"mapZoom":7,
"centerLat":[Latitude of the city as number only],
"centerLng":[Longitude of the city as number only],
"query":"path:\"$filename$\"",
"chosenMapSource":0,
"showLinks":false,
"linkColor":"red"}
```XXXXXX

## See also

1. [<Country Name> on Wikipedia](<Wikipedia link of the country>)
2. [<Country Name> on CIA World Factbook](<CIA World Factbook link of the country>)

## References
Add additional references as a list using the format:
'- [Source Name](URL of Source)'

"
In the line with the location, place the longitude coordinate directly after the comma following the latitude coordinate. Do not add an extra space

```

The strange-looking lines

```
> [!blank|float-right-small]
> ![Flag](<Link to the raw image of the flag of the country on Wikipedia>)
```

are just required for the Obsidian Modular CSS Layout mentioned in the prerequisites. The lines place the flag of the country to the right of the following text. In my opinion, that looks a little nicer than having the flag placed on a separate line.

## Conclusion

With the help of AI and an Obsidian plugin, it is possible to create initial note skeletons about countries that contain the required spatial information and an embedded map in a few minutes. That skeleton can be used as a good starting point for deeper exploration. The ChatGPT prompt can easily be enhanced and adjusted to your needs. 

Have fun!
