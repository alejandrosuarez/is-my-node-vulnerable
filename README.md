# is-my-node-vulnerable

This package helps ensure the security of your Node.js installation by checking for known vulnerabilities.
It compares the version of Node.js you have installed (`process.version`) to the [Node.js Security Database][]
and alerts you if a vulnerability is found.

## Usage

```
npx is-my-node-vulnerable
```

It's strongly recommended to include this as a step in the app CI.

### Output - When vulnerable


```console
$ node -v
v20.3.0
$ npx is-my-node-vulnerable


██████   █████  ███    ██  ██████  ███████ ██████
██   ██ ██   ██ ████   ██ ██       ██      ██   ██
██   ██ ███████ ██ ██  ██ ██   ███ █████   ██████
██   ██ ██   ██ ██  ██ ██ ██    ██ ██      ██   ██
██████  ██   ██ ██   ████  ██████  ███████ ██   ██


The current Node.js version (v20.3.0) is vulnerable to the following CVEs:

CVE-2023-30581: The use of proto in process.mainModule.proto.require() can bypass the policy mechanism and require modules outside of the policy.json definition
Patched versions: ^16.20.1 || ^18.16.1 || ^20.3.1
==================================================================================================================================================================================
```

### Output - When non-vulnerable

```console
$ node -v
v20.17.0
$ npx is-my-node-vulnerable


 █████  ██      ██           ██████   ██████   ██████  ██████         ██
██   ██ ██      ██          ██       ██    ██ ██    ██ ██   ██     ██  ██
███████ ██      ██          ██   ███ ██    ██ ██    ██ ██   ██         ██
██   ██ ██      ██          ██    ██ ██    ██ ██    ██ ██   ██     ██  ██
██   ██ ███████ ███████      ██████   ██████   ██████  ██████         ██

```

### Output - when end of life

```console
$ node -v
v15.14.0
$ npx is-my-node-vulnerable
██████   █████  ███    ██  ██████  ███████ ██████
██   ██ ██   ██ ████   ██ ██       ██      ██   ██
██   ██ ███████ ██ ██  ██ ██   ███ █████   ██████
██   ██ ██   ██ ██  ██ ██ ██    ██ ██      ██   ██
██████  ██   ██ ██   ████  ██████  ███████ ██   ██


v15.14.0 is end-of-life. There are high chances of being vulnerable. Please upgrade it.
```

End-of-Life versions don't keep track of recent security releases, therefore, it's considered vulnerable by default.

## API

This package also exports a function `isNodeVulnerable` to perform the check in runtime

```js
const { isNodeVulnerable } = require('is-my-node-vulnerable')

isNodeVulnerable('19.0.0') // true
```

Optionally you can define the platform with the argument `platform` to limit the scope. The available platforms are [the same values](https://nodejs.org/api/os.html#osplatform) available in for `os.platform()`.

```js
const { isNodeVulnerable } = require('is-my-node-vulnerable')

isNodeVulnerable('19.0.0', 'linux') // true
```

[Node.js Security Database]: https://github.com/nodejs/security-wg/tree/main/vuln


## Github Action

This package also provides a GitHub Action, just include the `node-version` in the yml as follows in order to check a specific version:

```yml
name: "Node.js Vulnerabilities"
on: 
  schedule:
    - cron: "0 0 * * *"

jobs:
  is-my-node-vulnerable:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Check Node.js
        uses: RafaelGSS/is-my-node-vulnerable@v1
        with:
          node-version: "18.14.1"
```

Optionally you can define the platform with the argument `platform` to limit the scope. The available platforms are [the same values](https://nodejs.org/api/os.html#osplatform) available in for `os.platform()`.

```yml
      - uses: actions/checkout@v3
      - name: Check Node.js
        uses: RafaelGSS/is-my-node-vulnerable@v1
        with:
          node-version: "18.14.1"
          platform: "linux"
```
