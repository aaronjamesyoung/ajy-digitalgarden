```dataviewjs
dv.table(
    ["cover", "title", "author", "series", "rating"],
	dv.pages('"93-books"')
	    .filter(b => {
	        let ret = false;
	        let year = false;
	        if(b.readdates) {
		        b.readdates.map(r => {
			        if(r.finished && r.finished.toString().includes("2022")) {
			            year = true;
			        }
			        return r;
		        });
	        }
	        if(b.shelf && (b.shelf === "read" || b.shelf === 'reading') && year) {
	            ret = true;
	        }
	        return ret;
	    })
		.sort(b => [b.author, b.series, b.seriesnumber, b.title])
	    .map(b => [
		    "![" + b.cover + "|80](" + b.cover + ")",
		    "[[" + b.file.name + "|" + b.title + "]]",
		    b.author,
		    b.series,
		    "⭐".repeat(b.rating)
		])
)
```