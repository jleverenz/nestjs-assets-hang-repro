# Issue Repro Project

See [PR #1922].

- Started with `npx @nestjs/cli new nestjs-assets-hang-repro`.
- Removed boilerplate service and controller.
- Added a single asset file.
- Switched to webpack.

For each commit in the repo, the obesrved behavior is expected: `npx nest build`
& `npx nest start` both exit properly.

For the last commit with webpack configured, `npx nest start` no longer exits.

## Test Cases

Command used: `npx nest start [--watch]`

- With `tsc` & no "watchAssets"
  - without `--watch`
    - starts up chokidar filewatcher
    - starts file copy before compilation (async without await)
    - assets copied at start-up only, assets not updated on watch
    - application exits
  - with `--watch`
    - starts up chokidar filewatcher
    - starts file copy before compilation (async without await)
    - assets copied at start-up only, assets *not* updated on watch
      - this includes after code changes that trigger a re-compile
    - application does not exit, in watch mode
- With `tsc` & top-level "watchAssets"
  - without `--watch`
    - same behavior as above
  - with `--watch`
    - starts up chokidar filewatcher
    - starts file copy before compilation (async without await)
    - assets copied at start-up, assets *are* updated on watch
    - application does not exit, in watch mode
- With `webpack` & no "watchAssets"
  - without `--watch`
    - same behavior as `tsc` case
    - **bug** does not exit after application exits, due to open chokidar watchers
  - with `--watch`
    - same behavior as `tsc` case
- With `webpack` & top-level "watchAssets"
  - without `--watch`
    - same behavior as above, including the **bug**
  - with `--watch`
    - same behavior as `tsc` case

The related [PR #1922] intends to remove the **bugs** above and align webpack
behavior with tsc behavior.

[PR #1922]: https://github.com/nestjs/nest-cli/pull/1922
