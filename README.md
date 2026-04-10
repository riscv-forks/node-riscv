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
