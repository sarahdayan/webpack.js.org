---
title: Module Variables
group: Modules
sort: 8
contributors:
  - skipjack
  - sokra
  - ahmehri
  - tbroadley
  - byzyk
  - EugeneHlushko
  - wizardofhogwarts
  - anikethsaha
  - chenxsan

related:
  - title: CommonJS
    url: https://en.wikipedia.org/wiki/CommonJS
  - title: Asynchronous Module Definition
    url: https://en.wikipedia.org/wiki/Asynchronous_module_definition
---

This section covers all __variables__ available in code compiled with webpack. Modules will have access to certain data from the compilation process through `module` and other variables.


### `module.loaded` (NodeJS)

This is `false` if the module is currently executing, and `true` if the sync execution has finished.


### `module.hot` (webpack-specific)

Indicates whether or not [Hot Module Replacement](/concepts/hot-module-replacement) is enabled and provides an interface to the process. See the [HMR API page](/api/hot-module-replacement) for details.


### `module.id` (CommonJS)

The ID of the current module.

``` javascript
module.id === require.resolve('./file.js');
```


### `module.exports` (CommonJS)

Defines the value that will be returned when a consumer makes a `require` call to the module (defaults to a new object).

``` javascript
module.exports = function doSomething() {
  // Do something...
};
```

W> This CANNOT be used in an asynchronous function.


### `exports` (CommonJS)

This variable is equal to the default value of `module.exports` (i.e. an object). If `module.exports` gets overwritten, `exports` will no longer be exported.

``` javascript
exports.someValue = 42;
exports.anObject = {
  x: 123
};
exports.aFunction = function doSomething() {
  // Do something
};
```


### `global` (NodeJS)

See [node.js global](https://nodejs.org/api/globals.html#globals_global).

For compatibility reasons webpack polyfills the `global` variable by default.


### `__dirname` (NodeJS)

Depending on the configuration option `node.__dirname`:

- `false`: Not defined
- `mock`: equal to `'/'`
- `true`: [node.js __dirname](https://nodejs.org/api/globals.html#globals_dirname)

If used inside an expression that is parsed by the Parser, the configuration option is treated as `true`.

### `import.meta.url`

Returns the absolute `file:` URL of the module.

__src/index.js__

```javascript
console.log(import.meta.url); // output something like `file:///path/to/your/project/src/index.js`
```

### `import.meta.webpack`

Returns the webpack version.

__src/index.js__

```javascript
console.log(import.meta.webpack); // output `5` for webpack 5
```

### `__filename` (NodeJS)

Depending on the configuration option `node.__filename`:

- `false`: Not defined
- `mock`: equal to `'/index.js'`
- `true`: [node.js __filename](https://nodejs.org/api/globals.html#globals_filename)

If used inside an expression that is parsed by the Parser, the configuration option is treated as `true`.


### `__resourceQuery` (webpack-specific)

The resource query of the current module. If the following `require` call was made, then the query string would be available in `file.js`.

``` javascript
require('file.js?test');
```

__file.js__

``` javascript
__resourceQuery === '?test';
```


### `__webpack_public_path__` (webpack-specific)

Equals the configuration option's `output.publicPath`.


### `__webpack_require__` (webpack-specific)

The raw require function. This expression isn't parsed by the Parser for dependencies.


### `__webpack_chunk_load__` (webpack-specific)

The internal chunk loading function. Takes two arguments:

- `chunkId` The id for the chunk to load.
- `callback(require)` A callback function called once the chunk is loaded.


### `__webpack_modules__` (webpack-specific)

Access to the internal object of all modules.


### `__webpack_hash__` (webpack-specific)

This variable is only available with the `HotModuleReplacementPlugin` or the `ExtendedAPIPlugin`. It provides access to the hash of the compilation.


### `__non_webpack_require__` (webpack-specific)

Generates a `require` function that is not parsed by webpack. Can be used to do cool stuff with a global require function if available.


### `__webpack_exports_info__` (webpack-specific)

In modules, `__webpack_exports_info__` is available to allow exports introspection:

- `__webpack_exports_info__` is always `true`

- `__webpack_exports_info__.<exportName>.used` is `false` when the export is known to be unused, `true` otherwise

- `__webpack_exports_info__.<exportName>.useInfo` is

    - `false` when the export is known to be unused
    - `true` when the export is known to be used
    - `null` when the export usage could depend on runtime conditions
    - `undefined` when no info is available

- `__webpack_exports_info__.<exportName>.provideInfo` is

    - `false` when the export is known to be not provided
    - `true` when the export is known to be provided
    - `null` when the export provision could depend on runtime conditions
    - `undefined` when no info is available

- Accessing the info from nested exports is possible: i. e. `__webpack_exports_info__.<exportName>.<exportName>.<exportName>.used`

### `DEBUG`  (webpack-specific)

Equals the configuration option `debug`.
