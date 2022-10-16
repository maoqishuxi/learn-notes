# Learn TypeScript

## Download TypeScript

### TypeScript in Your Project

via `npm`

TypeScript is available as a [package on the `npm` registry](https://www.npmjs.com/package/typescript) available as "typescript".

You will need a copy of Node.js as an environment to run the package. Then you use a dependency manager like `npm`, `yarn` or `pnpm` to download TypeScript into your project.

```
npm install typescript --save-dev
yarn add typescript --dev
```

All of these dependency managers `lockfiles`, ensuring that everyone on your team is using the same version of the language. You can then run the TypeScript compiler using one of the following commands:

```
npx tsc
yarn tsc
```

### Globally Installing TypeScript

It can be handy to have TypeScript available across all projects, often to test one-off ideas. Long-term, codebases should prefer a project-wide installation over a global install so that they can benefit from reproducible builds across different machines.

via `npm`

You can use `npm` to install TypeScript globally, this means that you can use the `tsc` command anywhere in your terminal.

To do this, run `npm install -g typescript`. This will install the latest version (currently 4.8).

An alternative is to use `npx` when you have to run `tsc` for one-off occasions.



