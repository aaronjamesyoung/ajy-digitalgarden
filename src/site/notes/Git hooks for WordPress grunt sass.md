---
{"dg-publish":true,"permalink":"/git-hooks-for-word-press-grunt-sass/"}
---


[[🗺️ Code\|🗺️ Code]]

Note: This is pretty old but served its function for copy/pasting back in the day. Not sure how many folks are starting new projects with Grunt nowadays.

It was helpful for me to split this into post-receive and post-checkout.

## post-receive

```bash
#!/bin/bash
git --work-tree=/var/www/html/wp-content/themes/asdf --git-dir=/root/asdf.git checkout -f
```

## post-checkout

```bash
#!/bin/bash
cd /var/www/html/wp-content/themes/asdf
grunt
chown -R www-data:www-data /var/www/html/*
```
