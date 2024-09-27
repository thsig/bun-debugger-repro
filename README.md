# Simple repro project for Bun debugger not attaching

Note: This project could probably have been made a bit simpler (i.e. one package without references in the `tsconfig.json` would probably be enough for the repro).

## Problem: Debugger doesn't attach when running compiled JS

Steps to reproduce:
1. Run `bun run build` from the repo root.
  * This will build the `cli` and `sdk` packages.
  * JSON and source maps are emitted to `cli/build` and `sdk/build`, respectively.
2. In VSCode, put a breakpoint on line 5 in `cli/index.ts` (by clicking on the left-hand side of the editor).
3. Using the command palette, run _Debug: Javascript Debug Terminal_.
3. In the JS debug terminal pane, cd to the repo root and run `bun run cli/build/index.js`.

On my end, at least, the debugger doesn't attach (and execution simply completes).

If I run `bun run cli/index.ts` (i.e. on the TypeScript file directly), the debugger _does_ stop at the breakpoint.

### Notes

* We're using `tsc` to compile here, but I was also not able to get the debugger to attach when running JS compiled by `bun build` .
* There are several reasons why we can't use `bun build`  during development, one being that we need different `tsconfig.json`-s for the different packages. Which is why we're using `tsc` to compile.
* It looks like the problem may be with Bun's source mapping, or the way Bun's debugger interacts with Bun's source mapping (that might explain why this works when running the TypeScript directly, but not when running the compiled JS when a corresponding source map is present next to it in the build dir).

