# `lts/lithium` for Node.js 26 LTS is unknown

Issue:  https://github.com/actions/setup-node/issues/1659

**Description:**

Attempting to use `lts/lithium` in `actions/setup-node` fails with:

```console
Error: Unable to find LTS release 'lithium' for Node version 'lts/lithium'.
```

**Action version:**

`action/setup-node@v7.1.0`

**Platform:**
- [X] Ubuntu
- [ ] macOS
- [ ] Windows

**Runner type:**
- [X] Hosted
- [ ] Self-hosted

**Tools version:**

N/A

**Repro steps:**

Specify `node-version: lts/lithium`

```yml
      - uses: actions/setup-node@v7.1.0
        with:
          node-version: lts/lithium
          check-latest: true
```

See https://github.com/MikeMcC399/actions-setup-node-test for runnable reproduction.

**Expected behavior:**

Specifying `lts/lithium` should install [Node.js 26.13.0](https://nodejs.org/en/blog/release/v26.13.0) (or later from the same release line, when available):

> 2026-10-28, Version 26.13.0 'Lithium' (LTS)
>
> Notable Changes
> This release marks the transition of Node.js 26.x into Long Term Support (LTS) with the codename 'Lithium'. It will continue to receive updates through to the end of April 2029.

https://github.com/actions/node-versions/blob/main/versions-manifest.json should label Node.js 26.13.0 as lts lithium

**Actual behavior:**

```console
Attempt to resolve LTS alias from manifest...
Error: Unable to find LTS release 'lithium' for Node version 'lts/lithium'.
```

---

## Resolution

Issue https://github.com/actions/setup-node/issues/1659 is now resolved.

https://github.com/actions/node-versions/blob/main/versions-manifest.json now starts with the following
and includes the key `"lts": "Lithium"` which is necessary to recognize Node.js 26.x as an LTS release:

```json
[
  {
    "version": "26.13.0",
    "stable": true,
    "lts": "Lithium",
    "release_url": "https://github.com/actions/node-versions/releases/tag/26.13.0-41378513395",
    "files": [
      {
        "filename": "node-26.13.0-darwin-arm64.tar.gz",
        "arch": "arm64",
        "platform": "darwin",
        "download_url": "https://github.com/actions/node-versions/releases/download/26.13.0-41378513395/node-26.13.0-darwin-arm64.tar.gz"
      },
```
