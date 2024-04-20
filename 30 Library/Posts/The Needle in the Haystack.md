---
up: 
tags:
  - Type/Post
created: 2024-04-03
categories:
  - "[[Medium Posts]]"
  - "[[Obsidian]]"
published: 2024-04-05
types:
  - "[[Posts]]"
---
# The Needle in the Haystack: Locating Non-Markdown files with the Obsidian Dataview plugin

![[Haystack.webp]]

Besides digital notes in Markdown format, my Obsidian vault contains numerous other files in various formats, ranging from PNGs and PDFs to Obsidian canvas files. While I can easily query my vault for Markdown notes using the Dataview plugin, this plugin does not treat other file formats as first class citizens.

This article is a quick tip to help you query your vault for non-Markdown files using Dataviews JavaScript API **Dataview JS**.

## What's the Problem?

After three years of using Obsidian as my Personal Knowledge Management system, I have accumulated more than 200 images and more than 30 Obsidian canvas files in my vault. And these files are scattered all over various directories, since I usually stored them in the folders I used for projects and trip planning.

I wanted to get a quick overview of these files and turned to the Dataview plugin to give me a list of these files.

```
table file.path where contains(file.path,".png")
```

That query returned "Dataview: No results to show for table query". I tried it with several different file-extensions, but none of the modified queries returned a result. Only one query worked as I expected:

```
table file.path where contains(file.path,".md")
```

After diving into the plugin documentation, I found out that this is what Dataview query language is intended to do: It provides methods to query Markdown notes and nothing else!

But wait: the Dataview plugin also provides a JavaScript API called "Dataview JS", that provides everything you need to handle Non-Markdown files too.

## Prerequisites

Before we start with the solution, you have to enable the Dataview JS API in the plugin settings:

![[Enable Dataview JS.png]]

The following sections include code examples, that only work if you enclose them in a dataviewjs comment block. To try them, simply copy the code and put the copy inside the comment block
![[Dataviewjs Comment block.png]]
## What's the Solution?

The Dataview JS API provides access to the Obsidian API that is needed to collect information about the Non-Markdown files in your vault. 

The steps to query for and to display these files are

1. Get all files in the Obsidian vault using the Obsidian API
2. Filter the result to contain only the files you want to display
3. Sort the result
4. Create a Dataview table with the filtered and sorted results

That's it! 

I am not very experienced with JavaScript, but even I was able to produce the following result in just a few minutes:

![[PNGs with Folder.png]]

The necessary code is straightforward

```
let files = app.vault.getFiles()
files = files
			.filter(file => file.extension === "png")
			.sort((a, b) => a.path.localeCompare(b.path))

dv.table(
	["Name","Folder"],
	files.map(file => [dv.fileLink(file.path), file.parent.path])
)
```

The first line of code gets the Obsidian Vault-API provided by Dataview JS (the variable "app.vault") and executes its method getFiles() to return all files in the vault. The result is stored in the variable files.
```
let files = app.vault.getFiles()
```

The next three lines filter the result to only contain PNG-files and then sorts them by their path.

```
files = files
			.filter(file => file.extension === "png")
			.sort((a, b) => a.path.localeCompare(b.path))
```

The final four lines 
1. produce a Dataview table with the headers "Name" and "Folder", 
2. iterate over the result (with the method "files.map(...)") and 
3. return the Obsidian link to the file (with "dv.fileLink(file.path)") and to the folder the file lives in (with "file.parent.path").

```
dv.table(
	["Name","Folder"],
	files.map(file => [dv.fileLink(file.path), file.parent.path])
)
```

The files returned by the call to app.vault.getFiles() return a JavaScript object with the following properties

- **name**: The complete filename
- **path**: The path to the file relative to the vaults root folder
- **parent**: The folder, the file belongs to
- **parent.name**: The name of the folder, the file belongs to
- **parent.path**: The path to the folder, the file belongs to
- **basename**: The name of the file without the extension
- **extension**: The file-extension of the file (e.g. "png")
- **stat.ctime**: The time of creation of the file as a Unix timestamp
- **stat.mtime**: The time of the last modification of the file as a Unix timestamp
- **stat.size**: The size of the file in bytes

All that information can be used for filtering, sorting and in the resulting Dataview table. Details can be viewed in the [Obsidian API documentation](https://docs.obsidian.md/Reference/TypeScript+API/TFile)

## Why is that so ugly?

When you also want to display the creation date, the modification date and the size of the file, you have to slightly adjust the dv.table(...) call:

```
let files = app.vault.getFiles()
files = files.filter(file => file.extension === "png")
			.sort((a, b) => a.path.localeCompare(b.path))

dv.table(
	["Name","Created", "Modified", "Size"],
	files.map(file => [dv.fileLink(file.path), file.stat.ctime, file.stat.mtime, file.stat.size])
)
```

However, the result looks a little ugly in the first place

![[UglyTableFormat.png]]

That is because the date values are in Unix timestamp format. That format can, however, easily be converted to a readable string using a little JavaScript helper function:

```
const DateFormatter = new Intl.DateTimeFormat()

function formatDate(value) {
	return DateFormatter.format(DateTime.fromMillis(value))
}
```

The display of the file size can also easily be beautified with JavaScript:

```
function formatFileSize(bytes) { 
	const sizes = ['Bytes', 'KB', 'MB', 'GB', 'TB']; 
	if (bytes === 0) return '0 Byte'; 
	const i = parseInt(Math.floor(Math.log(bytes) / Math.log(1024))); 
	if (i === 0) 
		return bytes + ' ' + sizes[i]; 
	return (bytes / Math.pow(1024, i)).toFixed(2) + ' ' + sizes[i]; 
}
```

The result then looks much better, at least in my opinion.

![[PrettyTableFormat.png]]

Here is the complete code required:

```
let files = app.vault.getFiles()

/** Format date values */
const DateFormatter = new Intl.DateTimeFormat()
function formatDate(value) {
	return DateFormatter.format(DateTime.fromMillis(value))
}

/** Format the file size */
function formatFileSize(bytes) { 
	const sizes = ['Bytes', 'KB', 'MB', 'GB', 'TB']; 
	if (bytes === 0) return '0 Byte'; 
	const i = parseInt(Math.floor(Math.log(bytes) / Math.log(1024))); 
	if (i === 0) 
		return bytes + ' ' + sizes[i]; 
	return (bytes / Math.pow(1024, i)).toFixed(2) + ' ' + sizes[i]; 
}

files = files.filter(file => file.extension === "png")
			.sort((a, b) => a.path.localeCompare(b.path))

dv.table(
	["Name","Created", "Modified", "Size"],
	files.map(file => [
		dv.fileLink(file.path), 
		formatDate(file.stat.ctime), 
		formatDate(file.stat.mtime),
		formatFileSize(file.stat.size)
		])
)
```

## But wait, isn't there More?

The mechanism can be slightly tweaked to produce a directory listing that also displays folders. Here is an example that shows a directory listing for the Screenshots-directory in my example vault.

![[Screenshots Dataview query.png]]
The required code is pretty straightforward:

```
const skipFolders = false
const path = "30 Library/Screenshots"
const fileIcon = "📄"
const folderIcon = "📁"

const header = ["Name", "Created", "Modified", "Size"]

/** Format date values */
const DateFormatter = new Intl.DateTimeFormat()
function formatDate(value) {
	return DateFormatter.format(DateTime.fromMillis(value))
}

/** Format the file size */
function formatFileSize(bytes) { 
	const sizes = ['Bytes', 'KB', 'MB', 'GB', 'TB']; 
	if (bytes === 0) return '0 Byte'; 
	const i = parseInt(Math.floor(Math.log(bytes) / Math.log(1024))); 
	if (i === 0) 
		return bytes + ' ' + sizes[i]; 
	return (bytes / Math.pow(1024, i)).toFixed(2) + ' ' + sizes[i]; 
}

async function getFileInfo(file) {
	const stat = await app.vault.adapter.stat(file.path)
	let ret = {
		"name": file.name,
		"path": file.path,
		"parent": file.parent,
		"type": stat.type,
		"ctime": formatDate(stat.ctime),
		"mtime": formatDate(stat.mtime),
		"size": formatFileSize(stat.size)
	}
	if (ret.type != "folder") {
		ret.sortname = "10-"+ret.name
		ret.basename = file.basename
		ret.extension = file.extension
		ret.link = dv.fileLink(file.path)
		ret.icon = fileIcon
	} else {
		ret.sortname = "00-"+ret.name
		ret.link = ret.name
		ret.icon = folderIcon
		ret.size = "--"
	}
	return ret
}

async function getFilesInFolder(path, skipDirectories) {
	let files = app.vault.getFolderByPath(path).children
	if (skipDirectories) {
		files = files.filter(file => file.stat !== undefined)
	}
	return Promise.all(files.map( file => getFileInfo(file)))
}


let files = await getFilesInFolder(path, skipFolders)
dv.table(header,files.sort((a, b) => a.sortname.localeCompare(b.sortname))
.map(file => [file.icon + " " + file.link, file.ctime, file.mtime, file.size]))
```

At the top of the file are 4 constants that configure the listing.

- **skipFolders**: defines whether directories are shown or not
- **path**: defines the path to the folder for the listing
- **fileIcon** and **folderIcon**: define the icons to display in front of the file- or folder name

If you want to display the listing for the current folder (the folder, the active note is located), use **dv.current().file.folder** instead of a fixed folder name. Dataview will substitute that value with the path of the current note.

## Summary

It is easy to query Non-Markdown files in an Obsidian vault by using the Dataview plugins JavaScript API. You do not have to be an experienced JavaScript programmer to use this API. 

If you are interested in an example vault using some sample scripts, feel free to download my simple sample vault from GitHub.


