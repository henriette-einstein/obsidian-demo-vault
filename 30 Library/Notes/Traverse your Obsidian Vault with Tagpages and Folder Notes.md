---
tags:
  - Type/Note
created: 2024-03-10
categories:
  - "[[Medium Posts]]"
  - "[[Obsidian]]"
published: 2024-03-12
---
# **Navigating the Depths of Your Digital Brain: Optimizing Obsidian with Tag Pages and Folder Notes**
![[MOCs.webp]]

Obsidian offers different ways to organize your vault. You can use folders, tags, bookmarks, links, Maps of Content (MOCs) and probably other methods too. In this post, I describe a combination of folders and tags to streamline the discovery of notes.

The popular [Bear app](https://bear.app/) relies solely on tags to organize notes and - in my opinion - that works just fine. Bear treats a tag just like a folder and since the app allows subtags, it is easy to define a directory-like structure. A tag **\#Obsidian/Dataview** is mapped to a virtual directory **/Obsidian/Dataview**. All used tags are displayed like a file system in the left sidebar of the application.

![[Bear Sidebar.png]]

A note can have multiple tags attached to it and can then be located under different "directories". A note with the tags **\#Obsidian/Dataview** and **\#Blog post** can be located under the virtual directories **/Obsidian/Dataview** and **/Blog post**. Clicking on a tag in Bear opens the corresponding virtual directory, which further streamlines the navigation. 

## Tags in Obsidian

In comparison to the Bear implementation, the use of tags in Obsidian is rather clumsy. Without additional community plugins, Obsidian displays a list of the tags used in the vault that includes the number of occurrences in the left sidebar. The list can optionally be sorted, displayed hierarchically and collapsed or expanded, that's it. 

![[Obsidian Tags Hierarchy.png]]

Clicking on a tag opens a linear search result in the right sidebar.

![[Obsidian Tags Result.png]]

The functionality can be improved by installing the **Tag Wrangler** community plugin. With this plugin, it is possible to rename an existing tag. This feature is provided out of the box by the Bear app.

## Tag Pages

Another feature implemented by Tag Wrangler are **tag pages**. A tag page is an arbitrary note that contains the tag-string as an alias in the YAML frontmatter of the note. When right-clicking on a tag in the sidebar, it is possible to create a new tag page or open an existing tag page.
![[Tag Wrangler Right Click.png]]

The following screenshot shows the frontmatter section of the tag page for the tag **\#Type/MOC.** The property **aliases** contains the tag and the file is therefore associated with the given tag.

![[YAML MOC tagpage.png]]
This tag page is a normal Markdown file and - with a little help by the Dataview plugin - can list all notes with the given tag. The Dataview code is quite simple:

```js
list
where econtains(file.etags, this.aliases[0])
sort file.name
```

Instead of the **list** command, the **table** command can also be used if you want to display more information about the notes.

```js
table created, file.folder as "Folder"
where econtains(file.etags, this.aliases[0])
sort file.name
```

Please note that I use the function **econtains** and not **contains**. 
- econtains lists only the note with an exact match
- contains lists all notes with a match, even if it is not the exact match

This is done intentionally. If I have a note with the tag **\#Person/Author** and a tag page that has an alias **\#Person**, econtains would not match, while contains does match. As a basic rule: 
- if you want the result to **exclude** pages associated with subtags, use **econtains** 
- if you want the result to **include** pages associated with subtags, use **contains**.

The tag pages are shown when you hover over a tag in your notes. They can also be accessed by performing a right-click on a tag in the Tags panel on the left sidebar and choosing the command **Open tag page**.
## Folder Notes

The tag pages can be stored anywhere in your Obsidian vault and under any name you like. However, using another plugin and a simple convention can enhance the user experience.

The community plugin **Folder notes** lets you attach notes to folders so that you can click on the name of a folder to open the note. With that plugin, it is possible to mimic the behavior of the Bear app by creating a directory structure in Obsidian and then attaching tag notes to every folder.

To mimic the behavior of the Bear application, apply the following conventions:

1. Create a folder structure in Obsidian that matches the tag hierarchy
The tags \#Person, \#Person/Author, \#Person/Politician and \#Location/City can, for example, be represented by the directory structure

```
- Person
	- Author
	- Politician
- Location
	- City
```
2. Create a tag page for every tag you want to use
In this example, create tag pages for \#Person, \#Person/Author, \#Person/Politician, \#Location and \#Location/City 
3. Attach the tag page to the corresponding folder. This can be done by giving the tag page the same filename as the folder and placing it in the corresponding folder
```
- Person -> Person Tag Page
	- Author -> Person/Author Tag Page
	- Politician -> Person/Politician Tag Page
- Location -> Location Tag Page
	- City -> Location/City Tag Page
```

After these steps, you can traverse your vault using tags by clicking on the appropriate folder, just as you would in the Bear app. If a note has more than one tag, it will appear in more than one folder.

## How does it look like in the end?

To illustrate the result of the above approach, here is a screenshot of this blog post in an otherwise empty vault. The actual file is placed in the folder **30 Library/Types/Notes**, and it has the three tags **\#Type/Note**, **\#Category/Medium** and **\#Category/Obsidian**.

![[Note in Vault.png]]

I created tag pages for all the category tags and attached them to the folders **Categories**, **Categories/Medium Posts** and **Categories/Obsidian** inside the folder **20 Tags**. When I now click on the appropriate folders, the post appears here as a result of the Dataview query.

Here in the folder **Categories/Medium Posts**

![[Note in Medium.png]]

And here in the folder **Categories/Obsidian**

![[Note in Obsidian.png]]
## But where do I put my Notes?

You can put your notes in any location inside the Obsidian vault. If you like the P.A.R.A. method, use the P.A.R.A. directory structure. If you are a hardcore Zettelkasten freak, throw all your notes in a single directory. Tag Pages and Folder notes just allow you an easy way of navigating your vault based on tags.

I prefer an approach similar to [Capacities](https://capacities.io) and group my notes based on their type, but that is the story of a different blog post.
## Summary
You can easily enhance the user experience of Obsidian with three community plugins:

- Tag Wrangler
- Folder notes and
- Dataview

Even though there is some initial effort required, I think the result justifies the setup costs. If you are interested in an example vault using this method, feel free to [download my simple sample vault](https://github.com/henriette-einstein/obsidian-demo-vault) from Github.
