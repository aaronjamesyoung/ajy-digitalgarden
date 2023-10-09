---
{"dg-publish":true,"dg-show-backlinks":true,"grade":3,"permalink":"/how-i-track-books-and-reading-with-obsidian/","dgShowBacklinks":true,"dgPassFrontmatter":true}
---

[[📘 Books\|📘 Books]]

I've been using Obsidian for all my notes for some time now, and I've fairly recently started tracking my book reading in Obsidian as well.

I never really got into Goodreads, and I used [BookWyrm](https://joinbookwyrm.com/) for some time, but I don't need the social features of those types of sites. However, if you are looking for a Goodreads replacement with social functionality, then either Bookwyrm or [Hardcover](https://hardcover.app/) are excellent choices. If you just want a personal reading log, Obsidian will work great.

I'm definitely not the first to figure out this kind of system and I've built it from many other sources, but hopefully it's helpful for me to compile my method here.

My system is somewhat unique in that it allows for rereads.

### Required Community plugins

* [Obsidian Book Search Plugin](https://github.com/anpigon/obsidian-book-search-plugin) - The core of this system is that you'll have a separate note in your vault for each book. This plugin makes this step easy. You can search for a book, and this plugin will insert a note in your vault with book metadata pre-populated from Google Books.
* [Dataview](https://github.com/blacksmithgu/obsidian-dataview) - Visualize your notes

Install the above plugins, then let's do some setup.

### Doing some setup

**Configure the Book Search plugin**:

First, set a folder where all your book notes will live. Second, create a new note to serve as a template for when you are adding book notes to your vault. Here is my template:

```
---
title: "{{title}}"
author: {{author}}
series: 
seriesnumber: 
rating: 
readdates:
- started: 
  finished: 
shelf: toread
list: 
publisher: {{publisher}}
publish: {{publishDate}}
pages: {{totalPage}}
isbn: {{isbn10}} {{isbn13}}
cover: <%=book.coverUrl ? `https://books.google.com/books/publisher/content/images/frontcover/${[...book.coverUrl.split("&")[0].matchAll(/id.?(.*)/g)][0][1]}?fife=w600-h900&source=gbs_api` : ''%>
dateCreated: {{date}}
---

![cover|150]({{coverUrl}})

## {{title}}

### Description

{{description}}
```

A few notes on the template:

* `series`, `seriesnumber`, `rating`, `readdates`, `shelf`, and `list` properties are not populated by the plugin. After adding a book note, you'll need to update these yourself.
* For `shelf`, you can choose what you want to do. I set it to one of: `toread`, `reading`, `read`, or `stopped`.
* `list` is meant to be a freeform field; I use it to create a list of [[Recommended Books\|Recommended Books]].
* `cover` has been updated from the default cover URL that the plugin provides. This version will give you large-format covers instead of small.
* For `rating`, I use a number (1-5). My dataview queries below convert these to star emojis.
* `readdates` is meant to accomodate multiple rereads. You would fill it out like this:

```
readdates:
- started: 2023-08-10
  finished: 2023-08-15
- started: 2023-09-05
  finished: 2023-09-12
```

... and so on. Unfortunately this doesn't play well with the Obsidian Properties UI that was released recently in 1.4, so you might want to switch to source view while editing these dates.

**Configure Dataview**

Make sure that DataviewJS queries are enabled.

### Start using it: Add a few books

* Run the Obsidian command "Book Search: Create new note" and search for a book.
* You'll see a list of book editions. Pick one. Probaby the first one.
* Obsidian will create a new note and show it to you. Update the series, seriesnumber, and any other relevant information in the properties.

### Visualize your books

OK, here's the dataview dump! I use separate notes in my vault for each of these queries. For each query I'll indicate whether it's a `dataview` or `dataviewjs` code block.

**Bookshelf: to read** `dataview`

```
TABLE WITHOUT ID
"![cover|80](" + cover + ")" AS "Cover",
title,
author, series
FROM "books"
WHERE shelf="toread"
SORT title ASC
```

* This displays the cover at 80px wide
* Assumes that your book notes are stored in a folder called "books"
* Throughout these examples, double check your shelf names.

**Bookshelf: currently reading** `dataview`

```
TABLE WITHOUT ID
"![cover|80](" + cover + ")" AS "Cover",
title,
author, series
FROM "books"
WHERE shelf="reading"
SORT started ASC
```

**Bookshelf: stopped reading** `dataview`

```
TABLE WITHOUT ID
"![cover|80](" + cover + ")" AS "Cover",
title,
author, series
FROM "books"
WHERE shelf="stopped"
SORT title ASC
```

When stopping a book, don't put a date in "finished" because it'll make your book display in this next query:

**Bookshelf: Read in 2023** `dataviewjs`

```
function renderReadDates(readdates) {
	let str = '';
	str += new Date(readdates.started).toLocaleDateString('en-us', { month:"short", day:"numeric", year: "numeric"});
	str += ' - ';
	str += new Date(readdates.finished).toLocaleDateString('en-us', { month:"short", day:"numeric", year: "numeric"});
	return str;
}

function fullBookList(dvarr, year) {
	const retArr = [];

    // Get only books read during the specified year
    // But if a book was reread during the year, list it twice
	dvarr.map(b => {
		if(b.readdates) {
			b.readdates.map(d => {
				if(new Date(d.finished).getFullYear() === year) {
					const book = Object.assign({}, b);
					book.readdates = d;
					retArr.push(book);
				}
				return d;
			});
		}
		return b;
	});

    // Sort by date finished
	retArr.sort((a,b) => {
		let ret = 0;
		if(a.readdates.finished.toString() > b.readdates.finished.toString()) {
			ret = 1;
		} else if(a.readdates.finished.toString() < b.readdates.finished.toString()) {
			ret = -1;
		}
		return ret;
	});
	
	return retArr;
}

// Function definitions finished, kick it off here and set your year:

const year = 2023;
const pages = dv.pages('"books"');
const expandedPages = dv.array(fullBookList(pages, year));

dv.table(
	["cover", "title", "author", "series", "read", "rating"],
	expandedPages.map(b => [
		"![" + b.cover + "|80](" + b.cover + ")",
		"[[" + b.file.name + "|" + b.title + "]]",
		b.author,
		b.series,
		renderReadDates(b.readdates),
		"⭐".repeat(b.rating)
	])
);
```

OK, here's where things get interesting. We're using DataviewJS to create a fancier query.

* The first function helps us render start/finished dates into more natural language than YYYY-MM-DD.
* We get our list of books, and filter them down to the books read during the specified year.
* But if we reread a book during the year, we will list it again
* Then we build a table.
* `dv.pages('"books"')` refers to the "books" folder where your book notes are kept.

See [[Books Read in 2023\|Books Read in 2023]].

**List the 5-star books from 2023** `dataview`

```
LIST WITHOUT ID
title + " (" + author + ")"
FROM "books"
WHERE rating=5 AND contains(string(readdates.finished), "2023")
```

**Show how many books you have read in 2023** `dataview`

```
LIST WITHOUT ID
"So far in 2023, I've read " + length(rows) + " books."
FROM "books"
WHERE contains(string(readdates.finished), "2023")
GROUP BY dateformat(finished, "yyyy")
```

**Display covers of books grouped by series** (excludes books not in a series) `dataviewjs`

```
let groups = dv.pages('"books"')
    .filter(b => b.series)
	.groupBy(b => b.series)
	.sort(b => b.series);

for(let group of groups) {
    dv.header(3, group.key)
    let rows = group.rows.sort(b => b.seriesnumber, 'asc')
        .map(b => `<img src="${b.cover}" style="height: 160px; margin-right: 6px; margin-bottom: 6px; border-radius: 4px;" />`)
        .join("");
    dv.el('div', rows);
}
```

From all these examples, I hope you can build other queries you might want.

To extend this system, you can use the Air Quotes plugin to take notes and quote from your book. You'll need an ebook copy of your book, but this plugin makes it easy to find and insert quotes from your book.