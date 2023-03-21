```
pnpm i
pnpm build
```

❌ Build fails with `[ERROR] Could not resolve "path/module/fs..."`

But if I revert back to an older version of SvelteKit, using the following commands, it builds successfully.

```
pnpm i @sveltejs/kit@1.0.0-next.589
pnpm build
```

✅ Build passes

Here is the error that I'm getting on recent version of SvelteKit.

```
> Using @sveltejs/adapter-cloudflare
✘ [ERROR] Could not resolve "path"

    .svelte-kit/output/server/entries/endpoints/sitemap.xml/_server.js:1:17:
      1 │ import path from "path";
        ╵                  ~~~~~~

  The package "path" wasn't found on the file system but is built into node. Are you trying to
  bundle for node? You can use "platform: 'node'" to do that, which will remove this error.

✘ [ERROR] Could not resolve "path"

    node_modules/.pnpm/@svelteness+kit-docs@1.1.2_svelte@3.57.0/node_modules/@svelteness/kit-docs/node/index.js:1:19:
      1 │ import __path from 'path';
        ╵                    ~~~~~~

  The package "path" wasn't found on the file system but is built into node. Are you trying to
  bundle for node? You can use "platform: 'node'" to do that, which will remove this error.

✘ [ERROR] Could not resolve "url"

    node_modules/.pnpm/@svelteness+kit-docs@1.1.2_svelte@3.57.0/node_modules/@svelteness/kit-docs/node/index.js:2:49:
      2 │ import { fileURLToPath as __fileURLToPath } from 'url';
        ╵                                                  ~~~~~

  The package "url" wasn't found on the file system but is built into node. Are you trying to bundle
  for node? You can use "platform: 'node'" to do that, which will remove this error.

✘ [ERROR] Could not resolve "module"

    node_modules/.pnpm/@svelteness+kit-docs@1.1.2_svelte@3.57.0/node_modules/@svelteness/kit-docs/node/index.js:3:49:
      3 │ import { createRequire as __createRequire } from 'module';
        ╵                                                  ~~~~~~~~

  The package "module" wasn't found on the file system but is built into node. Are you trying to
  bundle for node? You can use "platform: 'node'" to do that, which will remove this error.

✘ [ERROR] Could not resolve "fs"

    node_modules/.pnpm/@svelteness+kit-docs@1.1.2_svelte@3.57.0/node_modules/@svelteness/kit-docs/node/index.js:21045:46:
      21045 │ import { readFileSync as readFileSync2 } from "fs";
            ╵                                               ~~~~

  The package "fs" wasn't found on the file system but is built into node. Are you trying to bundle
  for node? You can use "platform: 'node'" to do that, which will remove this error.

✘ [ERROR] Could not resolve "process"

    node_modules/.pnpm/@svelteness+kit-docs@1.1.2_svelte@3.57.0/node_modules/@svelteness/kit-docs/node/index.js:21057:21:
      21057 │ import process2 from "process";
            ╵                      ~~~~~~~~~

  The package "process" wasn't found on the file system but is built into node. Are you trying to
  bundle for node? You can use "platform: 'node'" to do that, which will remove this error.

✘ [ERROR] Could not resolve "stream"

    node_modules/.pnpm/@svelteness+kit-docs@1.1.2_svelte@3.57.0/node_modules/@svelteness/kit-docs/node/index.js:21073:26:
      21073 │ import { Transform } from "stream";
            ╵                           ~~~~~~~~

  The package "stream" wasn't found on the file system but is built into node. Are you trying to
  bundle for node? You can use "platform: 'node'" to do that, which will remove this error.

✘ [ERROR] Could not resolve "crypto"

    node_modules/.pnpm/@svelteness+kit-docs@1.1.2_svelte@3.57.0/node_modules/@svelteness/kit-docs/node/index.js:21637:27:
      21637 │ import { createHash } from "crypto";
            ╵                            ~~~~~~~~

  The package "crypto" wasn't found on the file system but is built into node. Are you trying to
  bundle for node? You can use "platform: 'node'" to do that, which will remove this error.

error during build:
Error: Build failed with 8 errors:
.svelte-kit/output/server/entries/endpoints/sitemap.xml/_server.js:1:17: ERROR: Could not resolve "path"
node_modules/.pnpm/@svelteness+kit-docs@1.1.2_svelte@3.57.0/node_modules/@svelteness/kit-docs/node/index.js:1:19: ERROR: Could not resolve "path"
node_modules/.pnpm/@svelteness+kit-docs@1.1.2_svelte@3.57.0/node_modules/@svelteness/kit-docs/node/index.js:2:49: ERROR: Could not resolve "url"
node_modules/.pnpm/@svelteness+kit-docs@1.1.2_svelte@3.57.0/node_modules/@svelteness/kit-docs/node/index.js:3:49: ERROR: Could not resolve "module"
node_modules/.pnpm/@svelteness+kit-docs@1.1.2_svelte@3.57.0/node_modules/@svelteness/kit-docs/node/index.js:21045:46: ERROR: Could not resolve "fs"
...
    at failureErrorWithLog (/Users/davejeffery/code/test/svelte-prerender-repro/node_modules/.pnpm/esbuild@0.16.17/node_modules/esbuild/lib/main.js:1604:15)
    at /Users/davejeffery/code/test/svelte-prerender-repro/node_modules/.pnpm/esbuild@0.16.17/node_modules/esbuild/lib/main.js:1056:28
    at /Users/davejeffery/code/test/svelte-prerender-repro/node_modules/.pnpm/esbuild@0.16.17/node_modules/esbuild/lib/main.js:1001:67
    at buildResponseToResult (/Users/davejeffery/code/test/svelte-prerender-repro/node_modules/.pnpm/esbuild@0.16.17/node_modules/esbuild/lib/main.js:1054:7)
    at /Users/davejeffery/code/test/svelte-prerender-repro/node_modules/.pnpm/esbuild@0.16.17/node_modules/esbuild/lib/main.js:1166:14
    at responseCallbacks.<computed> (/Users/davejeffery/code/test/svelte-prerender-repro/node_modules/.pnpm/esbuild@0.16.17/node_modules/esbuild/lib/main.js:701:9)
    at handleIncomingPacket (/Users/davejeffery/code/test/svelte-prerender-repro/node_modules/.pnpm/esbuild@0.16.17/node_modules/esbuild/lib/main.js:756:9)
    at Socket.readFromStdout (/Users/davejeffery/code/test/svelte-prerender-repro/node_modules/.pnpm/esbuild@0.16.17/node_modules/esbuild/lib/main.js:677:7)
    at Socket.emit (node:events:537:28)
    at addChunk (node:internal/streams/readable:324:12)
 ELIFECYCLE  Command failed with exit code 1.
```
