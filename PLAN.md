Please implement a (rather unusual) CI system for node.js on RISC-V that

- Users can open a PR with a file that contains the github repo of a node.js repo and a commit.
  - The repo is optional and defaults to nodejs/node
  - The commit is mandarory and cannot be a reference name like `main`. We must ensure that it is a commit hash to ensure the safty of the self-hosted builder
  - Optionally they can specify the compiler, which should default to clang.
- Then this PR triggers a build of node.js and the tests if the build is successful.
- Both the build and tests runs on a native riscv64 linux runner. You must not use third-party actions or first-party github actions because they do not support riscv64.
    - You should expect all the build dependencies are already installed.
    - The self-hosted runner is persistent and you should clear previous states in the nodejs repo. The repo should be at `~/node-riscv` and you should clone the repo if it is not found.
- You should add a default PR template that says something like "- [ ] By opening this PR, I hereby confirm that I have reviewed the corresponding commits and they do not contain malicious code that could compromise the self-hosted runners."
- Please name this workflow `native-ci.yaml`, we plan to add `cross-ci` in the future as well.
- It should include a trigger that makes it possible to manually trigger workflow runs, which should build from the latest commit of main branch in nodejs/node
- You should package the built binary (node and mksnapshot) and upload them as artifacts to GitHub Actions.
- You should not introduce AI slop.
- You should add a README file documenting its usage.
- You should think twice before going to implementation because we cannot test this workflow before pushing it out.