# Issue Repro Project

- Started with `npx @nestjs/cli new nestjs-assets-hang-repro`.
- Removed boilerplate service and controller.
- Added a single asset file.
- Switched to webpack.

For each commit in the repo, the obesrved behavior is expected: `npx nest build`
& `npx nest start` both exit properly.

For the last commit with webpack configured, `npx nest start` no longer exits.
