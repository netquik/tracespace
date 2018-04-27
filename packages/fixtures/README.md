# tracespace fixtures

[![npm][npm-badge]][npm]

> Test fixtures for tracespace projects

This module is a collection of data, including real-world open-source Gerber and drill files, used to drive tests on tracespace projects. It's published to npm so that any tool can use this data.

## install

Please note: because this package is a collection of test fixtures, it may not follow semver and changes could be introduced in _any_ version bump that cause your tests to fail. **You should install an exact version.**

``` shell
npm install --save-dev --exact @tracespace/fixtures
# or
yarn add --dev --exact @tracespace/fixtures
```

## usage

```js
const fixtures = require('@tracespace/fixtures')
```

### gerber filenames

`gerber-filenames.json` is a JSON file with a collection of example Gerber / drill filenames along with what PCB layer type they represent organized by CAD package. This fixture is primarily used to test `whats-that-gerber`.

```js
const {gerberFilenames} = require('@tracepsace/fixtures')
// or
const gerberFilenames = require('@tracespace/fixtures/gerber-filenames.json')
```

Where `gerberFilenames` looks like:

```js
Array<{
  /** CAD package, e.g. "eagle" */
  cad: string,
  /** collection of example Gerber and/or drill filenames */
  files: Array<{
    /** filename */
    name: string,
    /** expected whats-that-gerber type */
    type: string
  }>
}>
```

For non-npm projects, this file is also available via [the unpkg CDN][unpkg]:

```shell
curl https://unpkg.com/@tracespace/fixtures@${version}/gerber-filenames.json
```

### example boards

`@tracespace/fixtures` has a collection of sample open-source PCB files to test rendering full circuit boards. This fixture is primarily used to test `pcb-stackup` and `pcb-stackup-core`.

```js
const {getBoards} = require('@tracespace/fixtures')

// async
getBoards((error, boards) => {
  if (error) return console.error(error)
  console.log(boards)
})

// sync
const boards = getBoards.sync()
```

Where `boards` looks like:

```js
Array<{
  /** name of the board */
  name: string,
  /** path to the board's manifest */
  filepath: string,
  /** array of individual board layers */
  layers: Array<{
    /** name of the layer */
    name: string,
    /** path to the layer's gerber / drill file */
    filepath: string,
    /** basename of filepath */
    filename: string,
    /** format of the file */
    format: 'gerber' | 'drill',
    /** whats-that-gerber type of the layer */
    type: string,
    /** file contents (i.e. results of fs.readFile on filepath) */
    contents: string
  }>
}>
```

### gerber specs

`@tracespace/fixtures` has a collection of simple Gerber and NC drill files to ensure proper rendering of different image primitives and structures. This fixture is primarily used to test `gerber-to-svg`.

```js
const {getGerberSpecs} = require('@tracespace/fixtures')

// async
getGerberSpecs((error, suites) => {
  if (error) return console.error(error)
  console.log(suites)
})

// sync
const suites = getGerberSpecs.sync()
```

Where `suites` looks like:

```js
Array<{
  /** name of the suite */
  name: string,
  /** array of individual specs in the suite */
  specs: Array<{
    /** name of the spec */
    name: string,
    /** category of the spec (matches suite's name) */
    category: string,
    /** path to the spec's gerber / drill file */
    filepath: string,
    /** file contents (i.e. results of fs.readFile on filepath) */
    contents: string,
    /** SVG string of the expected render */
    expected: string
  }>
}>
```

### render suite server

Work in progress

```js
const {server} = require('@tracespace/fixtures')

const app = server('suite name', getResults)

app.listen(9000, () => console.log('listening on http://localhost:9000'))

function getResults (done) {
  // get results somehow and pass them to done
}
```

[npm]: https://www.npmjs.com/package/@tracespace/fixtures
[npm-badge]: https://img.shields.io/npm/v/@tracespace/fixtures.svg?style=flat-square&maxAge=86400
[unpkg]: https://unpkg.com