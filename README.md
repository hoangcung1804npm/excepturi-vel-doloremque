# @hoangcung1804npm/excepturi-vel-doloremque

[![Build Status](https://circleci.com/gh/blackflux/@hoangcung1804npm/excepturi-vel-doloremque.png?style=shield)](https://circleci.com/gh/blackflux/@hoangcung1804npm/excepturi-vel-doloremque)
[![Test Coverage](https://img.shields.io/coveralls/blackflux/@hoangcung1804npm/excepturi-vel-doloremque/master.svg)](https://coveralls.io/github/blackflux/@hoangcung1804npm/excepturi-vel-doloremque?branch=master)
[![Dependabot Status](https://api.dependabot.com/badges/status?host=github&repo=blackflux/@hoangcung1804npm/excepturi-vel-doloremque)](https://dependabot.com)
[![Dependencies](https://david-dm.org/blackflux/@hoangcung1804npm/excepturi-vel-doloremque/status.svg)](https://david-dm.org/blackflux/@hoangcung1804npm/excepturi-vel-doloremque)
[![NPM](https://img.shields.io/npm/v/@hoangcung1804npm/excepturi-vel-doloremque.svg)](https://www.npmjs.com/package/@hoangcung1804npm/excepturi-vel-doloremque)
[![Downloads](https://img.shields.io/npm/dt/@hoangcung1804npm/excepturi-vel-doloremque.svg)](https://www.npmjs.com/package/@hoangcung1804npm/excepturi-vel-doloremque)
[![Semantic-Release](https://github.com/blackflux/js-gardener/blob/master/assets/icons/semver.svg)](https://github.com/semantic-release/semantic-release)
[![Gardener](https://github.com/blackflux/js-gardener/blob/master/assets/badge.svg)](https://github.com/blackflux/js-gardener)

Drop in replacement for [mocha](https://github.com/mochajs/mocha) to abstract commonly used test setups


## Install

Install with [npm](https://www.npmjs.com/):

    $ npm install --save @hoangcung1804npm/excepturi-vel-doloremque

## Usage

Drop-in extension for mocha by simply importing `describe` as below.

<!-- eslint-disable import/no-unresolved, import/no-extraneous-dependencies -->
```js
import { expect } from 'chai';
import { describe } from '@hoangcung1804npm/excepturi-vel-doloremque';

describe('Testing some stuff', /* { ...options }, */ () => {
  it('Testing a thing', () => {
    expect(3 * 7).to.equal(21);
  });
});
```

Please see tests for further usage examples.

### Custom Cassette Logic

### delayConnection

Delay the connection by a certain number of ms.

### delayBody

Delay the response body by a certain number of ms.

### Function Kwargs

#### dir

Type: `string`

The tmp directory for this test. Only available when `useTmpDir` is set.

#### recorder

Type: `object`

Can be called to interact with the currently captured logs. Exposes the following functions:
- `get(level = null)`:  Returns array of recorded logs. Can be restricted by passing in the log level.
- `reset()`: Reset currently captured logs
- `verbose(flag: boolean)`: Set verbosity mode of capture (if the original logger function is called)

 Only available when `record` option is used.

#### capture

Type: `function`

Utility function that takes a function as an argument, calls it and expects it to raise an error. The raised error is returned. If no error is raised an assertion error is thrown instead.

#### fixture

Type: `function`

Utility function that can be used to load test fixtures from the `fixtureFolder`.

Internally this uses [smart-fs](https://www.npmjs.com/package/smart-fs) to determine how a file extension is loaded.

If the fixture is unique, the file extensions is not required.

### CLI Args

#### nock-heal

Used to heal nock recordings. This is useful when the body of (some) recordings is outdated or the recording order is invalid.
Can be used in the following ways:

- `--nock-heal`: Will try to heal ordering of nock cassette recordings
- `--nock-heal prune`: Will remove unmatched recordings from nock cassette
- `--nock-heal headers`: Will try to heal request headers of nock cassette recordings
- `--nock-heal body`: Will try to heal bodies of nock cassette recordings
- `--nock-heal path`: Will try to heal paths of nock cassette recordings
- `--nock-heal response`: Will try to heal responses
- `--nock-heal record`: Will record the next unmatched request
- `--nock-heal stub`: Will stub the next unmatched request
- `--nock-heal magic`: Shorthand for `headers,body,path,response`

Notes:
- Different flags can be combined as e.g. `--nock-heal body,path`
- Healing cassettes containing out-of-order requests can result in undesired behaviour

### Options

#### useTmpDir

Type: `boolean`<br>
Default: `false`

When set to true, a fresh temporary directory is set up for each test. The directory is cleaned up after the test run has completed.

#### useNock

Type: `boolean`<br>
Default: `false`

When set to true, all requests are automatically nocked. The recording files are automatically created relative to the current test file.

#### nockFolder

Type: `string`<br>
Default: `$FILENAME__cassettes`

Used to customize the folder name that contains the nock cassettes. This can be useful when multiple describe in
the same file use nock.

#### nockModifiers

Type: `object`<br>
Default: `{}`

Used to define modifiers that can be used in cassettes.
E.g. can be used to make encoded response bodies more readable.
See tests for how to use in detail.

### nockStripHeaders

Type: `boolean`<br>
Default: `false`

When set to true, all headers are stripped when requests are recorded.

### nockReqHeaderOverwrite

Type: `object`<br>
Default: `{}`

Can be used to overwrite `reqheaders` in recordings. Cassette files are only updated when changed.

#### fixtureFolder

Type: `string`<br>
Default: `$FILENAME__fixtures`

Used to customize the folder name that contains the test fixtures.
Fixtures can be loaded by calling `fixture(FIXTURE_NAME)`.

#### envVarsFile

Type: `string`<br>
Default: `$FILENAME.env.yml`

Used to customize the name of the file that environment variables are loaded from, if it exists.

To allow overwriting of environment variables, prefix the name of the environment variable with `^`.

#### envVars

Type: `object`<br>
Default: -

Used to declare environment variables per describe. Overwrites environment variables
loaded from `envVarsFile` (if allowed).

To allow overwriting of environment variables, prefix the name of the environment variable with `^`.

#### clearCache

Type: `boolean`<br>
Default: `true`

Known accessed caches will be cleared after test has executed when set to `true`.

#### timestamp

Type: `number|string`<br>
Default: -

Set timestamp to freeze time to. Will modify the result of e.g. `new Date()`.

#### record

Type: `object`<br>
Default: -

Expects logger (e.g. `console`) to be passed in and captures input,
which can be accessed by using `recorder` from within the test.

#### cryptoSeed

Type: `string`<br>
Default: -

When set, randomization is overwritten and consistent per test using the provided seed.

#### cryptoSeedReseed

Type: `boolean`<br>
Default: `false`

When set to true, all random functions are re-seeded. This results in reduced randomness.

Only allowed when cryptoSeed is provided.

#### timeout

Type: `number`<br>
Default: -

Set the timeout for all tests in the suite.
