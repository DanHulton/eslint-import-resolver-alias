# eslint-import-resolver-alias

This is a simple Node.js module import resolution plugin for [`eslint-plugin-import`](https://www.npmjs.com/package/eslint-plugin-import), which supports native Node.js module resolution, module alias/mapping and custom file extensions.

NOTE: This package is an update of [the original](https://github.com/johvin/eslint-import-resolver-alias), modified to allow for arrays of aliases.  At this time, it is provided as a convenience only, until the [original maintainer](https://github.com/johvin) is able to return and address the PR containing this code.  Any issues or bug reports not explicitly related to the alias array modifications are better addressed against the original repository.


## Installation

Prerequisites: Node.js >=4.x and corresponding version of npm.

```shell
npm install eslint-plugin-import eslint-import-resolver-alias-array --save-dev
```


## Usage

Pass this resolver and its parameters to `eslint-plugin-import` using your `eslint` config file, `.eslintrc` or `.eslintrc.js`.

```js
// .eslintrc.js
module.exports = {
  settings: {
    'import/resolver': {
      'alias-array': {
        map: [
          ['babel-polyfill', 'babel-polyfill/dist/polyfill.min.js'],
          ['helper', './utils/helper'],
          ['material-ui/DatePicker', '../custom/DatePicker'],
          ['material-ui', 'material-ui-ie10'],
          ['lib', ['app/lib', 'common/lib']]
        ],
        extensions: ['.ts', '.js', '.jsx', '.json']
      }
    }
  }
};
```

Note:

- The alias config object contains two properties, `map` and `extensions`, both of which are array types
- The item of `map` array is also array type which contains 2 string
    + The first string represents the alias of module name or path
    + The second string represents the actual module name or path
    + The second string may also be an array that is evaluated in order, with the first found used as resolved
- The `map` item `['helper', './utils/helper']` means that the modules which match `helper` or `helper/*` will be resolved to `./utils/helper` or `./utils/helper/*` which are located relative to the `process current working directory` (almost the project root directory). If you just want to resolve `helper` to `./utils/helper`, use `['^helper$', './utils/helper']` instead. See [issue #3](https://github.com/johvin/eslint-import-resolver-alias/issues/3)
- The order of 'material-ui/DatePicker' and 'material-ui' cannot be reversed, otherwise the alias rule 'material-ui/DatePicker' does not work
- The default value of `extensions` property is `['.js', '.json', '.node']` if it is assigned to an empty array or not specified

*If the `extensions` property is not specified, the config object can be simplified to the `map` array.*

```js
// .eslintrc.js
module.exports = {
  settings: {
    'import/resolver': {
      'alias-array': [
        ['babel-polyfill', 'babel-polyfill/dist/polyfill.min.js'],
        ['helper', './utils/helper'],
        ['material-ui/DatePicker', '../custom/DatePicker'],
        ['material-ui', 'material-ui-ie10']
      ]
    }
  }
};
```

When the config is not a valid object (such as `true`), the resolver falls back to native Node.js module resolution.

```js
// .eslintrc.js
module.exports = {
  settings: {
    'import/resolver': {
      alias: true
    }
  }
};
```

## CHANGELOG

[`CHANGELOG`](./CHANGELOG.md)

## References

- eslint-plugin-import/no-extraneous-dependencies
- eslint-plugin-import/no-unresolved
- eslint-module-utils/resolve
- resolve
- eslint-import-resolver-node
