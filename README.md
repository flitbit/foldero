# foldero

A light-weight, flexible, folder-to-object loader utility.

> _rhymes with bolero_

## Use

`foldero` will load files from the folder you specify and construct an object with properties corresponding to each loaded file. The default behavior looks for javascript files (.js and .json) and loads those using nodejs' `require` method. You can override most of `foldero`'s behavior through its options.

`foldero` exports a single function.

### #foldero(path [, options])

The `foldero` function takes a `path` and an optional `options` argument.

* `path` {string} - either the file or folder path that `foldero` should inspect when constructing a result object.
* `options` {object} - an optional object specifying the following:
  * `calculateName` {function} - a callback function used to name the properties on the result object.
  * `ignore` {string} - names a file path to ignore
  * `index` {string} default='index.js' - the name of file that should be treated as a folder's _index_ file. If specified, wherever a file with this name appears in the folder hierarchy, only that file is loaded from the folder.
  * `loader` {function} - a callback funciton used to _load_ each file that has been whitelisted.
  * `noIndex` {boolean} - indicates that processing of index files should be skipped.
  * `relative` {string} - a base-path specifying how relative paths in the `path` argument are interpreted. Without this option all paths are treated as absolute paths.
  * `whitelist` {function|string} - a callback function used to whitelist files to be loaded. If a string is specified it is transformed into a RegEx and used to whitelist files.

#### Basics

##### Load Path as Object

```javascript
var foldero = require('foldero');

var stuff = foldero('/path/to/your/stuff');
```

##### Load Relative Path as Object

```javascript
var foldero = require('foldero');

var stuff = foldero('./stuff', { relative: __dirname });
```

##### Load Recursively

```javascript
var foldero = require('foldero');

var stuff = foldero('./stuff', { relative: __dirname, recurse: true });
```

##### Load Non-Javascript Stuff

```javascript
var path = require('path');
var fs = require('fs');

var foldero = require('foldero');

var pages = foldero('./pages', {
  relative: __dirname,
  whitelist: "(.*/)*.+\.(htm|html|HTM|HTML)$",
  calculateName: path.basename.bind(path),
  loader: function loadAsString(file) {
    return fs.readFileSync(file, 'utf8');
  },
  recurse: true
  });
```

## [License](https://github.com/flitbit/foldero/raw/master/LICENSE)

