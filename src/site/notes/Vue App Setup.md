---
{"dg-publish":true,"permalink":"/vue-app-setup/"}
---


[[🗺️ Code\|🗺️ Code]]

## Vue UI options

* Package Manager: npm
* Manually Select Features
    * Vue 3
    * Router
    * CSS Pre-processors (SCSS / Dart)
    * Linter / Formatter (ESLint + Prettier)
* Add Dependencies
    * onchange (dev dependency)

## Configuration after creating project

### vue.config.js

Add line `publicPath: '',`

### package.json

In eslint "Rules" section, add:

```json
"prettier/prettier": [
    "error",
    { "singleQuote": true }
]
```

At end, add new section:

```json
"prettier": {
    "trailingComma": "es5",
    "tabWidth": 2,
    "singleQuote": true
}
```

In "scripts" section, add:

```json
"format": "prettier . --write",
"prettier-watch": "onchange '**/*' -- prettier --write --ignore-unknown {{changed}}"
```

## SCSS setup

* In assets/scss, start w/ `style.scss` and set up a bunch of imports. *TODO - document better*

### Water CSS

* [Water.css](https://watercss.kognise.dev/)
* [GitHub - kognise/water.css: A drop-in collection of CSS styles to make simple websites just a little nicer](https://github.com/kognise/water.css)
* Import into style.scss

## Vue router apache htaccess

For apache, you need to use a .htaccess rewrite to get vue router to work correctly.

https://router.vuejs.org/guide/essentials/history-mode.html#example-server-configurations

```apacheconf
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteRule ^index\.html$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.html [L]
</IfModule>
```

If you're in a subfolder:

```apacheconf
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /subdirectoryName
RewriteRule ^subdirectoryName/index\.html$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /subdirectoryName/index.html [L]
</IfModule>
```

https://gist.github.com/Prezens/f99fd28124b5557eb16816229391afee