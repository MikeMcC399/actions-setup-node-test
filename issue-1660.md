# `latest` installs v27.0.0-alpha.0.0.0 not 26.13.0

Issue:  https://github.com/actions/setup-node/issues

- follow-on from comment https://github.com/actions/setup-node/issues/1657#issuecomment-6518244012
- relates to https://github.com/nodejs/node/issues/67135

**Description:**

Selecting version `latest` causes Node.js 27.0.0-alpha.0.0.0 to be installed.

This is not the `latest` version as defined by the Node.js documentation [Release types](https://github.com/nodejs/node#release-types).

https://nodejs.org/download/release/latest/ contains builds for Node.js 26.13.0 which is the Active LTS version, and the highest non-alpha release.

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

Specify `node-version: latest`

```yml
      - name: Install Node.js latest
        uses: actions/setup-node@v7.1.0
        with:
          node-version: latest
          check-latest: true
```

See [setup-node-latest.yml](https://github.com/MikeMcC399/actions-setup-node-test/blob/main/.github/workflows/setup-node-latest.yml) for runnable reproduction.

**Expected behavior:**

Specifying `latest` should install [Node.js 26.13.0](https://nodejs.org/en/blog/release/v26.13.0) (or later from the same Node.js 26.x release line, when available).

---
[Release types](https://github.com/nodejs/node#release-types) says:

> https://nodejs.org/download/release/
>
> The latest directory is an alias for the latest Current release.

---

```console
Index of /download/release/latest/
../
docs/                                                             -                   -
win-arm64/                                                        -                   -
win-x64/                                                          -                   -
SHASUMS256.txt.asc                                 28-Oct-2026 13:01               4.1 KB
SHASUMS256.txt.sig                                 28-Oct-2026 13:01                566 B
SHASUMS256.txt                                     28-Oct-2026 17:41               3.2 KB
node-v26.13.0-aix-ppc64.tar.gz                     28-Oct-2026 12:59                83 MB
node-v26.13.0-arm64.msi                            28-Oct-2026 12:59                33 MB
node-v26.13.0-darwin-arm64.tar.gz                  28-Oct-2026 12:59                58 MB
node-v26.13.0-darwin-arm64.tar.xz                  28-Oct-2026 12:59                31 MB
node-v26.13.0-darwin-x64.tar.gz                    28-Oct-2026 12:59                60 MB
node-v26.13.0-darwin-x64.tar.xz                    28-Oct-2026 12:59                33 MB
node-v26.13.0-headers.tar.gz                       28-Oct-2026 12:59                10 MB
node-v26.13.0-headers.tar.xz                       28-Oct-2026 12:59               584 KB
node-v26.13.0-linux-arm64.tar.gz                   28-Oct-2026 12:59                63 MB
node-v26.13.0-linux-arm64.tar.xz                   28-Oct-2026 12:59                33 MB
node-v26.13.0-linux-ppc64le.tar.gz                 28-Oct-2026 12:59                65 MB
node-v26.13.0-linux-ppc64le.tar.xz                 28-Oct-2026 12:59                33 MB
node-v26.13.0-linux-s390x.tar.gz                   28-Oct-2026 12:59                65 MB
node-v26.13.0-linux-s390x.tar.xz                   28-Oct-2026 12:59                33 MB
node-v26.13.0-linux-x64-musl.tar.gz                28-Oct-2026 12:59                63 MB
node-v26.13.0-linux-x64-musl.tar.xz                28-Oct-2026 12:59                34 MB
node-v26.13.0-linux-x64.tar.gz                     28-Oct-2026 12:59                63 MB
node-v26.13.0-linux-x64.tar.xz                     28-Oct-2026 12:59                34 MB
node-v26.13.0-win-arm64.7z                         28-Oct-2026 12:59                24 MB
node-v26.13.0-win-arm64.zip                        28-Oct-2026 12:59                37 MB
node-v26.13.0-win-x64.7z                           28-Oct-2026 12:59                26 MB
node-v26.13.0-win-x64.zip                          28-Oct-2026 12:59                41 MB
node-v26.13.0-x64.msi                              28-Oct-2026 12:59                37 MB
node-v26.13.0.pkg                                  28-Oct-2026 12:59               105 MB
node-v26.13.0.tar.gz                               28-Oct-2026 12:59               127 MB
node-v26.13.0.tar.xz                               28-Oct-2026 12:59                63 MB
```

**Actual behavior:**

`actions/setup-node` installs `v27.0.0-alpha.0.0.0` when asked for `latest`

```console
Run actions/setup-node@v7.1.0
  with:
    node-version: latest
    check-latest: true
    token: ***
    package-manager-cache: true
getting latest node version...
getting latest node version...
Attempt to resolve the latest version from manifest...
Failed to resolve version v27.0.0-alpha.0.0.0 from manifest
Attempting to download v27.0.0-alpha.0.0.0...
Not found in manifest. Falling back to download directly from Node
Acquiring 27.0.0-alpha.0.0.0 - x64 from https://nodejs.org/dist/v27.0.0-alpha.0.0.0/node-v27.0.0-alpha.0.0.0-linux-x64.tar.gz
Extracting ...
/usr/bin/tar xz --strip 1 --warning=no-unknown-keyword --overwrite -C /home/runner/work/_temp/aa316193-6949-4603-bec6-c73e4bd2ceb6 -f /home/runner/work/_temp/57b2047f-1960-4aad-a93d-830eee1e5900
Adding to the cache ...
Done
Environment details
  node: v27.0.0-alpha.0.0.0
  npm: 12.1.0
  yarn: 1.22.22
```
