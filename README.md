# keyv-file [<img width="100" align="right" src="https://rawgit.com/lukechilds/keyv/master/media/logo.svg" alt="keyv">](https://github.com/lukechilds/keyv)

> IPFS storage adapter for Keyv, using json to serialize data fast and small.

[![Build Status](https://travis-ci.org/cjrutherford/keyv-file.svg?branch=master)](https://travis-ci.org/cjrutherford/keyv-file)
[![npm](https://img.shields.io/npm/v/keyv-file.svg)](https://www.npmjs.com/package/keyv-file)

IPFS storage adapter for [Keyv](https://github.com/lukechilds/keyv).

TTL functionality is handled internally by interval scan, don't need to panic about expired data take too much space.

## Install

```shell
npm install --save keyv keyv-ipfs
```

## Usage

### Using with keyv
```js
const Keyv = require('keyv')
const KeyvIpfs = require('keyv-ipfs')

const keyv = new Keyv({
  store: new KeyvIpfs()
});
// More options with default value:
const customKeyv = new Keyv({
  store: new KeyvIpfs({
    filename: `${os.tmpdir()}/keyv-file/default-rnd-${Math.random().toString(36).slice(2)}.json`, // the file path to store the data
    expiredCheckDelay: 24 * 3600 * 1000, // ms, check and remove expired data in each ms
    writeDelay: 100, // ms, batch write to disk in a specific duration, enhance write performance.
    encode: JSON.stringify, // serialize function
    decode: JSON.parse // deserialize function
  })
})
```

### Using directly

```ts
import KeyvIpfs, { makeField } from 'keyv-ipfs'

class Kv extends KeyvFile {
  constructor() {
    super({
      filename: './db.json'
    })
  }
  someField = makeField(this, 'field_key')
}

export const kv = new Kv

kv.someField.get(1) // empty return default value 1
kv.someField.set(2) // set value 2
kv.someField.get() // return saved value 2
kv.someField.delete() // delete field
```

## License

MIT
