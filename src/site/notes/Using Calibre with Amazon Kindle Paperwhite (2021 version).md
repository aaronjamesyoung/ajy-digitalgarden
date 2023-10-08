---
{"dg-publish":true,"dg-show-backlinks":false,"grade":3,"permalink":"/using-calibre-with-amazon-kindle-paperwhite-2021-version/","dgPassFrontmatter":true}
---


I recently got a new Kindle - the newest Paperwhite released in 2021 - and took a fair amount of time to figure out the best way to handle sideloaded books. Here are my thoughts:

## The current situation

I have an epub library which I manage on my Linux laptop with [calibre - E-book management](https://calibre-ebook.com/). I don't have a ton of them. I have been trying to standardize my library as epub-only, but obviously that won't work with a Kindle.

Amazon DOES now support sending an epub to Kindle via email, or (I think) the desktop "Send to Kindle" app. The Kindle then does an onboard conversion to a compatible format. The "Send to Kindle" app isn't available for Linux, and I'd prefer to use a data cable to connect my Kindle to my laptop anyway. So I need to use Calibre to convert my books to a kindle-compatible format.

## Kindle format options

There are three formats that Kindle can use: mobi, azw3, and kfx. Others will be able to give more detail about these, but here is how it stands from my point of view:

* mobi files are outdated and have limited formatting. Use one of the newer specs.
* kfx is the newest format, and fullest-featured. But Calibre can't output to kfx on its own - it requires an addon as well as the "Send to Kindle" desktop app - which doesn't work on Linux.
* azw3 is a good, current format and can be sideloaded to Kindle. The drawback is that the 9-page "page flip" feature only works with kfx

I will be using azw3, so I set Calibre's default conversion output format to azw3. I also set the default page profile to "Kindle Oasis" since the hint indicates that profile works for the newest Paperwhite as well.

## Metadata and covers

There's a well-known issue with the Kindle removing cover art from sideloaded books. It looks something like this:

* Calibre uploads a book to the kindle, including cover art
* The Kindle phones home to Amazon and tries (and fails) to fetch the "correct" cover art
* The Kindle falls back to a generic cover for all your sideloaded books

To combat this, each book that you sideload needs to have an ASIN number in its metadata. In Calibre, you need to use the "Identifiers" metadata field to add this number in the format `amazon:B003P2WO5E`.

Metadata is... inconsistent in my collection. I went through each book (actually in groups) and used Calibre to download metadata for the books from the internet. This had varying degrees of success. As I went, I ensured that each book had its Amazon ASIN number - and if not, I went to Amazon's site and grabbed it for manual entry.

The Kindle may fetch different covers from Amazon than the ones in my Calibre library. With some more work they could be made to match up. [Here is one way to do so (third comment).](https://www.reddit.com/r/Calibre/comments/ru5pgz/comment/hqxzz3t/?utm_source=reddit&utm_medium=web2x&context=3)

After getting the ASIN entered for each book in my library, I converted them all to azw3 for upload.

## Syncing content

I use the included Kindle data cable to send books from Calibre to my Kindle.

I do not consider this to be a 2-way sync. My Calibre library is the source of the data and Kindle is essentially a client. I don't keep them synchronized with each other, but I upload books from Calibre to Kindle when I decide to read them.

I very rarely turn on wifi on my Kindle.