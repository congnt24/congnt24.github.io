---
layout: post
title: "NPM: Build and publish a npm package"
categories: backend
image: "http://techkids.vn/blog/wp-content/uploads/2017/06/NPM.jpg"
tags: [backend, nodejs]
---

## I. Concept
- **Package:** is a folder containing a program described by a package.json file.
- **Module:** is a folder with a package.json file, wich contain main field, that enables loading the module with require() in a Node program.
- The **node_modules** folder is the place Node.js looks for modules.

<!--more-->

## II. Setup
In this article, I will create a module to replace all special characters in input string to ascii characters. Ex: Â->A, Ê->E...
### 1. Create an empty project
Use *npm init* to create an empty npm package or use *npm init --yes* for lazy init.
```bash
mkdir node-deburr && cd node-deburr
npm init --yes
```
Your package.json file should look like this:

```javascript
{
  "name": "node-deburr",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```
By default, a main field will be created and it's entry point to your module. User can install your package by *require("node-deburr")*

You can edit some field to make is personal suck as: description, author...
### 2. Create entry point
Now we have a package.json in folder, but we have not had entry point file to our module. Name of that file will be the value of main field in package.json file.
Now we create index.js file
```bash
touch index.js
vi index.js
```
Here you can write the main code of your module. For this module, i'll have something like that in index.js file.

```javascript
var reLatin = /[\xc0-\xd6\xd8-\xf6\xf8-\xff\u0100-\u017f]/g;
var deburredLetters = {
    // Latin-1 Supplement block.
    '\xc0': 'A',  '\xc1': 'A', '\xc2': 'A', '\xc3': 'A', '\xc4': 'A', '\xc5': 'A',
    '\xe0': 'a',  '\xe1': 'a', '\xe2': 'a', '\xe3': 'a', '\xe4': 'a', '\xe5': 'a',
    '\xc7': 'C',  '\xe7': 'c',
    '\xd0': 'D',  '\xf0': 'd',
    '\xc8': 'E',  '\xc9': 'E', '\xca': 'E', '\xcb': 'E',
    '\xe8': 'e',  '\xe9': 'e', '\xea': 'e', '\xeb': 'e',
    '\xcc': 'I',  '\xcd': 'I', '\xce': 'I', '\xcf': 'I',
    '\xec': 'i',  '\xed': 'i', '\xee': 'i', '\xef': 'i',
    '\xd1': 'N',  '\xf1': 'n',
    '\xd2': 'O',  '\xd3': 'O', '\xd4': 'O', '\xd5': 'O', '\xd6': 'O', '\xd8': 'O',
    '\xf2': 'o',  '\xf3': 'o', '\xf4': 'o', '\xf5': 'o', '\xf6': 'o', '\xf8': 'o',
    '\xd9': 'U',  '\xda': 'U', '\xdb': 'U', '\xdc': 'U',
    '\xf9': 'u',  '\xfa': 'u', '\xfb': 'u', '\xfc': 'u',
    '\xdd': 'Y',  '\xfd': 'y', '\xff': 'y',
    '\xc6': 'Ae', '\xe6': 'ae',
    '\xde': 'Th', '\xfe': 'th',
    '\xdf': 'ss',
    // Latin Extended-A block.
    '\u0100': 'A',  '\u0102': 'A', '\u0104': 'A',
    '\u0101': 'a',  '\u0103': 'a', '\u0105': 'a',
    '\u0106': 'C',  '\u0108': 'C', '\u010a': 'C', '\u010c': 'C',
    '\u0107': 'c',  '\u0109': 'c', '\u010b': 'c', '\u010d': 'c',
    '\u010e': 'D',  '\u0110': 'D', '\u010f': 'd', '\u0111': 'd',
    '\u0112': 'E',  '\u0114': 'E', '\u0116': 'E', '\u0118': 'E', '\u011a': 'E',
    '\u0113': 'e',  '\u0115': 'e', '\u0117': 'e', '\u0119': 'e', '\u011b': 'e',
    '\u011c': 'G',  '\u011e': 'G', '\u0120': 'G', '\u0122': 'G',
    '\u011d': 'g',  '\u011f': 'g', '\u0121': 'g', '\u0123': 'g',
    '\u0124': 'H',  '\u0126': 'H', '\u0125': 'h', '\u0127': 'h',
    '\u0128': 'I',  '\u012a': 'I', '\u012c': 'I', '\u012e': 'I', '\u0130': 'I',
    '\u0129': 'i',  '\u012b': 'i', '\u012d': 'i', '\u012f': 'i', '\u0131': 'i',
    '\u0134': 'J',  '\u0135': 'j',
    '\u0136': 'K',  '\u0137': 'k', '\u0138': 'k',
    '\u0139': 'L',  '\u013b': 'L', '\u013d': 'L', '\u013f': 'L', '\u0141': 'L',
    '\u013a': 'l',  '\u013c': 'l', '\u013e': 'l', '\u0140': 'l', '\u0142': 'l',
    '\u0143': 'N',  '\u0145': 'N', '\u0147': 'N', '\u014a': 'N',
    '\u0144': 'n',  '\u0146': 'n', '\u0148': 'n', '\u014b': 'n',
    '\u014c': 'O',  '\u014e': 'O', '\u0150': 'O',
    '\u014d': 'o',  '\u014f': 'o', '\u0151': 'o',
    '\u0154': 'R',  '\u0156': 'R', '\u0158': 'R',
    '\u0155': 'r',  '\u0157': 'r', '\u0159': 'r',
    '\u015a': 'S',  '\u015c': 'S', '\u015e': 'S', '\u0160': 'S',
    '\u015b': 's',  '\u015d': 's', '\u015f': 's', '\u0161': 's',
    '\u0162': 'T',  '\u0164': 'T', '\u0166': 'T',
    '\u0163': 't',  '\u0165': 't', '\u0167': 't',
    '\u0168': 'U',  '\u016a': 'U', '\u016c': 'U', '\u016e': 'U', '\u0170': 'U', '\u0172': 'U',
    '\u0169': 'u',  '\u016b': 'u', '\u016d': 'u', '\u016f': 'u', '\u0171': 'u', '\u0173': 'u',
    '\u0174': 'W',  '\u0175': 'w',
    '\u0176': 'Y',  '\u0177': 'y', '\u0178': 'Y',
    '\u0179': 'Z',  '\u017b': 'Z', '\u017d': 'Z',
    '\u017a': 'z',  '\u017c': 'z', '\u017e': 'z',
    '\u0132': 'IJ', '\u0133': 'ij',
    '\u0152': 'Oe', '\u0153': 'oe',
    '\u0149': "'n", '\u017f': 's'
};
var flatWord = function (str, simple) {
    if (simple === true) {
        str = str.replace(reLatin, function(key){return deburredLetters[key]});
    }
    // language=JSRegexp
    str = str.replace(/[àáạảãâầấậẩẫăằắặẳẵ]/g, "a");
    str = str.replace(/[èéẹẻẽêềếệểễ]/g, "e");
    str = str.replace(/[ìíịỉĩ]/g, "i");
    str = str.replace(/[òóọỏõôồốộổỗơờớợởỡ]/g, "o");
    str = str.replace(/[ùúụủũưừứựửữ]/g, "u");
    str = str.replace(/[ỳýỵỷỹ]/g, "y");
    str = str.replace(/đ/g, "d");

    str = str.replace(/[ÀÁẠẢÃÂẦẤẬẨẪĂẰẮẶẲẴ]/g, "A");
    str = str.replace(/[ÈÉẸẺẼÊỀẾỆỂỄ]/g, "E");
    str = str.replace(/[ÌÍỊỈĨ]/g, "I");
    str = str.replace(/[ÒÓỌỎÕÔỒỐỘỔỖƠỜỚỢỞỠ]/g, "O");
    str = str.replace(/[ÙÚỤỦŨƯỪỨỰỬỮ]/g, "U");
    str = str.replace(/[ỲÝỴỶỸ]/g, "Y");
    str = str.replace(/Đ/g, "D");
    var rsAstralRange = '\\ud800-\\udfff',
        rsComboMarksRange = '\\u0300-\\u036f',
        reComboHalfMarksRange = '\\ufe20-\\ufe2f',
        rsComboSymbolsRange = '\\u20d0-\\u20ff',
        rsComboRange = '[' + rsAstralRange + rsComboMarksRange + reComboHalfMarksRange + rsComboSymbolsRange + ']';
    str = str.replace(RegExp(rsComboRange, 'g'), '');
    return str
};
module.exports = flatWord;
```

In **index.js** file, I export a function to **module.exports**: flatWord.
## III. Test

But we haven't publish to NPM yet. So, to test this module, we create a new project first, then using npm-link for testing purpose.

```bash
cd ..
mkdir test-node-deburr && cd test-node-deburr
npm init --yes
touch index.js
```

Then edit index.js file to the following code:

```javascript
var deburr = require('node-deburr');
console.log(deburr("hôm nay là thứ mấy?"))
// output: hom nay la thu may?
```

To run test project

```bash
node index.js
```

But you will see errors like below because you haven't publish your module to npm yet.

```bash
module.js:545
    throw err;
    ^

Error: Cannot find module 'node-deburr'
    at Function.Module._resolveFilename (module.js:543:15)
    at Function.Module._load (module.js:470:25)
    at Module.require (module.js:593:17)
    at require (internal/module.js:11:18)
    at Object.<anonymous> (/Volumes/Data/Projects/Backend/test-node-deburr/index.js:1:76)
    at Module._compile (module.js:649:30)
    at Object.Module._extensions..js (module.js:660:10)
    at Module.load (module.js:561:32)
    at tryModuleLoad (module.js:501:12)
    at Function.Module._load (module.js:493:3)
```

So, to testing before publish your module to npm, you could use npm-link in module folder then install that module to test folder.
```bash
cd ../node-deburr
npm link
cd ../test-node-deburr
npm link node-deburr
node index.js
# - hom nay la thu may?
```

## IV. Publish
After testing you module locally. It's time to publish your module to the world.
To publish you have to had a npmjs.com account. If you not have any account, you can access to npmjs.com or using **npm adduser** to create your account.
```bash
npm adduser
```
Then input your username/password to create new npm account.
After successful in createing account, npm will auto login for you. Use **npm whoami** to see which account is logged.

To publish, use **npm publish**. To update version, use **npm version <update_type>** where update_type can be **path, minor or major**
```bash
cd ../node-deburr
npm publish
# - node-deburr@1.0.0
npm version patch
# - v1.0.1
npm version minor
# - v1.1.0
npm version major
# - v2.0.0
npm publish
# - node-deburr@2.0.0
```

## V. Local dependency

To install module locally, we create another project called *local-node-deburr* then copy node-deburr module to then using **npm install <path to node-deburr>** to install.
```bash
cd ..
mkdir local-node-deburr && cd local-node-deburr
npm init -y
cp -R ../node-deburr .
npm install ./node-deburr
cat package.json
# -...
# - "dependencies": {
# -    "node-deburr": "file:node-deburr"
# -  }
```
Now you can using syntax:
```javascript
require('node-deburr');
```

# REFERENCE
https://github.com/congnt24/node-deburr
