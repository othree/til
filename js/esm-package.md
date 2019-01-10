To serve both CommonJS module and ES module in an NPM package at the same time.
And use ES Module for modern bundle tools such as [webpack][] or [rollup][]. We should:

First, remove the file extension from the `main` entry in the `package.json` and
provide both `js` and `mjs` file. `js` file is CommonJS module. `mjs` file is 
ES module. This is for node's [experimental support][mjs] of ES module.

Second, add `module` entry (ref: [pkg.module][]). The value is the ES module file.
And I suggest not use the mjs file directly. Copy the `.mjs` file, rename it and
use `js` as its file extension. This is for bundle tool.

Example: https://github.com/othree/smartypants.js/blob/master/package.json#L4-L6

ps. `jsnext:main` is used by webpack 2.

[webpack]:https://webpack.js.org/
[rollup]:https://rollupjs.org/guide/en
[pkg.module]:https://github.com/rollup/rollup/wiki/pkg.module
[mjs]:https://nodejs.org/api/esm.html
