---
up: "[[Images]]"
created: 2024-04-04
description: Screenshots in this vault
---

```dataviewjs
const skipFolders = false
const path = dv.current().file.folder
const fileIcon = "📄"
const folderIcon = "📁"

const header = ["Name", "Created", "Modified", "Size"]

const DateFormatter = new Intl.DateTimeFormat()

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
		"folder": file.parent.path,
		"type": stat.type,
		"ctime": DateFormatter.format(DateTime.fromMillis(stat.ctime)),
		"mtime": DateFormatter.format(DateTime.fromMillis(stat.mtime)),
		"size": formatFileSize(stat.size)
	}
	if (ret.type != "folder") {
		ret.sortname = "11-"+ret.name
		ret.basename = file.basename
		ret.extension = file.extension
		ret.link = dv.fileLink(file.path)
		ret.icon = ":LiFile:"
	} else {
		ret.sortname = "00-"+ret.name
		ret.link = ret.name
		ret.icon = ":LiFolder:"
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

