# Santi's DICT Client

[![Build Status][workflow badge]][repo actions]
[![npm homepage][npm badge]][npm home]
[![GitHub stars][stars badge]][repo url]
[![License][license badge]][repo url]
[![Bundlephobia stats][bundlephobia badge]][bundlephobia url]

[workflow badge]: https://github.com/santi100a/dict-client/actions/workflows/ci.yml/badge.svg
[npm badge]: https://img.shields.io/npm/v/@santi100a/dict-client
[stars badge]: https://img.shields.io/github/stars/santi100a/dict-client.svg
[license badge]: https://img.shields.io/github/license/santi100a/dict-client.svg
[bundlephobia badge]: https://img.shields.io/bundlephobia/min/@santi100a/dict-client
[npm home]: https://npmjs.org/package/@santi100a/dict-client
[repo actions]: https://github.com/santi100a/dict-client/actions
[repo url]: https://github.com/santi100a/dict-client
[bundlephobia url]: https://bundlephobia.com/package/@santi100a/dict-client@latest

## ⚠ WARNING
This package uses the `node:net` module for TCP connections and, thus, **only works on Node.js** for now.

## Description

This NPM package is a promise-based DICT (RFC 2229) client used to retrieve word definitions from a DICT server.
Since it needs a raw TCP socket in order to function properly, it is based on the `node:net` module and, thus,
only works on Node.js for now.

## Installation

- Via NPM: `npm install @santi100a/dict-client`
- Via Yarn: `yarn add @santi100a/dict-client`
- Via PNPM: `pnpm install @santi100a/dict-client`

## Minimal usage example

```typescript
import { DictClient } from '@santi100a/dict-client'; // TypeScript, OR
const { DictClient } = require('@santi100a/dict-client'); // CommonJS


(async function () {
    const client = new DictClient();

    // Must be called before sending any command
    const banner = await client.connect(new URL('dict://dict.org:2628'));
    
    console.log('Connected to DICT server');
    console.log('Message:', banner.message); // => dict.dict.org dictd 1.12.1/rf on Linux 4.19.0-10-amd64
	// Identify the client (optional but recommended)
	await client.client('dict-client-example/1.0');

	// Define a word using all available databases
	const result = await client.define('*', 'example');

	for (const def of result.definitions) {
		console.log(`\n[${def.dictionary}] ${def.dictionaryDescription}`); // => [gcide] The Collaborative ...
		console.log(def.definition); // => Example \Ex*am"ple\, n. [A later form for ensample, fr. L. exemplum...]
	}

	// Always close the connection
	await client.quit(); // client.disconnect() works as well
})().catch(console.error);
```

For more details, check the JSDocs.
