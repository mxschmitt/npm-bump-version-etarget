# Repro

1. `npm i`
1. `npm --no-git-tag-version --workspaces version "1.42.0"` (version which does not exist on NPM)
1. Edit `packages/playwright-test/package.json` bump there the `playwright-core` dependency to `=1.42.0`.
1. `npm install`

Expected: Works. Project gets successfully installed

Actual: Error gets thrown:

```shell
➜  npm-bump-version-etarget git:(master) ✗ npm i
npm ERR! code ETARGET
npm ERR! notarget No matching version found for playwright-core@=1.20.1.
npm ERR! notarget In most cases you or one of your dependencies are requesting
npm ERR! notarget a package version that doesn't exist.

npm ERR! A complete log of this run can be found in:
npm ERR!     /Users/max/.npm/_logs/2021-10-25T14_07_42_079Z-debug.log
```

Logs:

```log
0 verbose cli [
0 verbose cli   '/Users/max/.nvm/versions/node/v14.17.1/bin/node',
0 verbose cli   '/Users/max/.nvm/versions/node/v14.17.1/bin/npm',
0 verbose cli   'i'
0 verbose cli ]
1 info using npm@8.1.1
2 info using node@v14.17.1
3 timing npm:load:whichnode Completed in 0ms
4 timing config:load:defaults Completed in 2ms
5 timing config:load:file:/Users/max/.nvm/versions/node/v14.17.1/lib/node_modules/npm/npmrc Completed in 16ms
6 timing config:load:builtin Completed in 16ms
7 timing config:load:cli Completed in 2ms
8 timing config:load:env Completed in 0ms
9 timing config:load:file:/Users/max/development/npm-bump-version-etarget/.npmrc Completed in 0ms
10 timing config:load:project Completed in 1ms
11 timing config:load:file:/Users/max/.npmrc Completed in 1ms
12 timing config:load:user Completed in 1ms
13 timing config:load:file:/Users/max/.nvm/versions/node/v14.17.1/etc/npmrc Completed in 0ms
14 timing config:load:global Completed in 0ms
15 timing config:load:validate Completed in 1ms
16 timing config:load:credentials Completed in 0ms
17 timing config:load:setEnvs Completed in 1ms
18 timing config:load Completed in 24ms
19 timing npm:load:configload Completed in 24ms
20 timing npm:load:setTitle Completed in 19ms
21 timing npm:load:setupLog Completed in 0ms
22 timing config:load:flatten Completed in 3ms
23 timing npm:load:cleanupLog Completed in 2ms
24 timing npm:load:configScope Completed in 0ms
25 timing npm:load:projectScope Completed in 1ms
26 timing npm:load Completed in 50ms
27 timing arborist:ctor Completed in 1ms
28 timing arborist:ctor Completed in 0ms
29 timing idealTree:init Completed in 110ms
30 timing idealTree:userRequests Completed in 0ms
31 silly idealTree buildDeps
32 timing idealTree:#root Completed in 1ms
33 timing idealTree:packages/playwright-core Completed in 0ms
34 silly fetch manifest playwright-core@=1.20.1
35 http fetch GET 200 https://registry.npmjs.org/playwright-core 56ms (cache hit)
36 silly placeDep packages/playwright-test playwright-core@ REPLACE for: @playwright/test@1.20.1 want: =1.20.1
37 timing idealTree:packages/playwright-test Completed in 89ms
38 timing idealTree:packages/playwright-test/node_modules/playwright-core Completed in 0ms
39 timing idealTree:buildDeps Completed in 91ms
40 timing idealTree:fixDepFlags Completed in 1ms
41 timing idealTree Completed in 204ms
42 timing command:install Completed in 215ms
43 verbose type version
44 verbose stack playwright-core: No matching version found for playwright-core@=1.20.1.
44 verbose stack     at module.exports (/Users/max/.nvm/versions/node/v14.17.1/lib/node_modules/npm/node_modules/npm-pick-manifest/index.js:209:23)
44 verbose stack     at /Users/max/.nvm/versions/node/v14.17.1/lib/node_modules/npm/node_modules/pacote/lib/registry.js:118:26
44 verbose stack     at async Arborist.[nodeFromEdge] (/Users/max/.nvm/versions/node/v14.17.1/lib/node_modules/npm/node_modules/@npmcli/arborist/lib/arborist/build-ideal-tree.js:1061:19)
44 verbose stack     at async Arborist.[buildDepStep] (/Users/max/.nvm/versions/node/v14.17.1/lib/node_modules/npm/node_modules/@npmcli/arborist/lib/arborist/build-ideal-tree.js:930:11)
44 verbose stack     at async Arborist.buildIdealTree (/Users/max/.nvm/versions/node/v14.17.1/lib/node_modules/npm/node_modules/@npmcli/arborist/lib/arborist/build-ideal-tree.js:216:7)
44 verbose stack     at async Promise.all (index 1)
44 verbose stack     at async Arborist.reify (/Users/max/.nvm/versions/node/v14.17.1/lib/node_modules/npm/node_modules/@npmcli/arborist/lib/arborist/reify.js:149:5)
44 verbose stack     at async Install.install (/Users/max/.nvm/versions/node/v14.17.1/lib/node_modules/npm/lib/install.js:170:5)
45 verbose cwd /Users/max/development/npm-bump-version-etarget
46 verbose Darwin 20.6.0
47 verbose argv "/Users/max/.nvm/versions/node/v14.17.1/bin/node" "/Users/max/.nvm/versions/node/v14.17.1/bin/npm" "i"
48 verbose node v14.17.1
49 verbose npm  v8.1.1
50 error code ETARGET
51 error notarget No matching version found for playwright-core@=1.20.1.
52 error notarget In most cases you or one of your dependencies are requesting
52 error notarget a package version that doesn't exist.
53 verbose exit 1
```

Probably because it tries to resolve playwright-core over the NPM Registry instead of the workspace packages.
