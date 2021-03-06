Tests
=====

Dependencies are managed with [Yarn](https://yarnpkg.com/), so you'll
have to [install it](https://yarnpkg.com/en/docs/install) first.

Then, to install dev dependencies:

```bash
yarn
```

We use [`mocha`][1] for testing cozy-desktop, the options are in
[`test/mocha.opt`][2].

There are several levels of tests.


Unit tests
----------

For testing a class in isolation, method per method:

```bash
npm run test-unit
```


Integration tests
-----------------

:warning: **Important**: the integration tests remove all the files and folders
on the Cozy! We recommend using the default repository with
`COZY_DESKTOP_DIR=tmp`.

Integration tests require that you have the Cozy dev VM up (it means CouchDB, a
data-system and a proxy up and running) and that the files application is
accessible on the 9121 port. It's also expected that a user is registered with
the [test password](../test/helpers/password.js).

To test the communication between cozy-desktop and a remote cozy stack (proxy,
data-system, files, etc.)

```bash
COZY_DESKTOP_DIR=tmp npm run test-integration
```


Options
-------

It's possible to launch unit and integration tests:

```bash
COZY_DESKTOP_DIR=tmp npm test
```

To run a specific set of tests (here testing pouch)

```bash
NODE_ENV=test node_modules/.bin/mocha test/unit/pouch.js
```

For more logs you can activate debug logs:

```bash
DEBUG=true COZY_DESKTOP_DIR=tmp npm run test
```


Coverage
--------

You can enable coverage metrics for any npm command with the
[`coverage.sh`][3] script.

Examples:

```bash
./scripts/coverage.sh npm run test
./scripts/coverage.sh npm run test-unit
```

Please note that code coverage is only measured for unit tests.
Integration tests have another purpose, so they are deliberately excluded,
even when running `./scripts/coverage.sh npm run test-integration`
explicitely.

Please also note that we don't measure coverage on the GUI for now.

Implementation details:

1. `npm run test-unit-coverage` wraps the `mocha` command with
   [nyc][3] to compile the code with [babel-plugin-istanbul][3].
the [appropriate option][3].
2. `babel-plugin-istanbul` inserts instrumentation code when compiling from
   EcmaScript to JavaScript
3. The mocha tests are run and generate an lcov-style report (including
   HTML output)
4. Finally, when run on the CI, we [tell Travis](../.travis.yml) to upload the report to the
   [Codecov][5] service.


[1]:  https://mochajs.org/
[2]:  ../test/mocha.opts
[3]: https://github.com/istanbuljs/nyc
[4]: https://github.com/istanbuljs/babel-plugin-istanbul
[5]: https://codecov.io/gh/cozy-labs/cozy-desktop
