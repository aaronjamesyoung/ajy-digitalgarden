```dataviewjs
function renderReadDates(readdates) {
	let str = '';
	for(var i=0; i<readdates.length; i++) {
	    if(new Date(readdates[i].finished).getFullYear() === 2023) {
			str += '* ';
			str += new Date(readdates[i].started).toLocaleDateString('en-us', { month:"long", day:"numeric", year: "numeric"});
			str += ' - ';
			str += new Date(readdates[i].finished).toLocaleDateString('en-us', { month:"long", day:"numeric", year: "numeric"});
			str += "\n";
		}
	}
	return str;
}

dv.table(
    ["cover", "title", "author", "series", "read", "rating"],
	dv.pages('"93-books"')
	    .filter(b => {
	        let ret = false;
	        let year = false;
	        if(b.readdates) {
		        b.readdates.map(r => {
			        if(r.finished && r.finished.toString().includes("2023")) {
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
		.sort(b => b.readdates[b.readdates.length-1].finished)
	    .map(b => [
		    "![" + b.cover + "|80](" + b.cover + ")",
		    b.title,
		    b.author,
		    b.series,
		    renderReadDates(b.readdates),
		    "⭐".repeat(b.rating)
		])
)
```