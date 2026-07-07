# node-riscv

CI system for building and testing Node.js on native RISC-V (riscv64) hardware.

## Submitting a build request

1. Edit the `build-config` file with your desired configuration:

   ```
   repo=nodejs/node
   commit=abc0123456789abcdef0123456789abcdef012345
   compiler=clang
   ```

   | Field | Required | Default | Description |
   |-------|----------|---------|-------------|
   | `repo` | No | `nodejs/node` | GitHub repository (`owner/name`) |
   | `commit` | **Yes** | — | Full 40-character commit SHA. Branch/tag names are rejected. |
   | `compiler` | No | `clang` | `clang` or `gcc` |

2. Open a pull request with your changes to `build-config`.

3. The CI workflow builds Node.js at the specified commit, runs the test suite on a native riscv64 runner, and uploads the resulting binaries as workflow artifacts.

## Artifacts

Successful builds upload a tarball containing:

- `node` — the Node.js binary
- `mksnapshot` / `node_mksnapshot` — the V8 snapshot tool

## Cross-compiled builds

The `Cross RISC-V Build` workflow builds Node.js v24 release tags on an x64
GitHub-hosted runner for `linux-riscv64`.

For pull request validation, edit `cross-build-config` in the PR:

```
tag=v24.18.0
local_revision=1
```

| Field | Required | Default | Description |
|-------|----------|---------|-------------|
| `tag` | **Yes** | — | Node.js v24 release tag, for example `v24.4.0` |
| `local_revision` | No | `1` | Positive local rebuild number, used in artifact names |
| `jobs` | No | `nproc` | Parallel build jobs |

Pull request builds upload the tarballs as a GitHub Actions artifact instead of
creating a GitHub Release.

For release publishing, run the same workflow manually from GitHub Actions with:

| Input | Description |
|-------|-------------|
| `tag` | Node.js v24 release tag, for example `v24.4.0` |
| `local_revision` | Positive local rebuild number, used in the GitHub release tag and tarball variation |
| `jobs` | Parallel build jobs, empty = `nproc` |

Manual builds upload the artifact and then publish the same files to this
repository's GitHub Releases.

The workflow uses Chromium's `debian_trixie_riscv64_sysroot`, clang target
compilers, and Ninja. It applies every `.patch` and `.diff` file in
`v24-patches/` before building.

For `tag=v24.4.0` and `local_revision=1`, the GitHub release tag is
`v24.4.0-riscv64.1` and the uploaded tarballs are named like
`node-v24.4.0-linux-riscv64-local1.tar.xz`. The release also includes
`SHASUMS256.txt` for the uploaded tarballs.
