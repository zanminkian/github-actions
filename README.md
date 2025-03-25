# github-actions

Reusable github actions.

## Release Action

Release npm package (based on [changesets/action](https://github.com/changesets/action)).

### Highlights

- ✅ Npm Provenance.
- 📦 Support 4 package managers: `npm`, `pnpm`, and `yarn`.

### Usages

1. Prepare 2 scripts and `engines.node` in the root `package.json`.

```json
{
  "scripts": {
    "test": "echo 'pass'", // Replace it with your own test script
    "release": "pnpm -r publish && changeset tag" // Replace `pnpm -r publish` with your own publish script
  },
  "engines": {
    "node": ">=22.0.0"
  }
}
```

2. Create a repository secret named `TOKENS` in `https://github.com/YOUR_NAME/YOUR_REPO/settings/secrets/actions`. It is a JSON string. Each key-value pair will become an environment variable. For example:

```json
{"NPM_TOKEN": "your_npm_token"}
```


3. Create a github workflow file.

```sh
mkdir -p .github/workflows && \
echo 'name: Release

on:
  push:
    branches:
      - main
      - prerelease

jobs:
  release:
    permissions:
      contents: write
      id-token: write
      pull-requests: write
    uses: zanminkian/github-actions/.github/workflows/release.yml@v1
    secrets:
      TOKENS: ${{ secrets.TOKENS }}' > .github/workflows/release.yml
```

4. Commit the github workflow file and push to the main branch.

5. Example projects:
   - [fenge](https://github.com/zanminkian/fenge): A TypeScript project publishing to npm.
   - [@rnm/tscx](https://github.com/rnmjs/tscx): A tiny tsc wrapper with many convenient options.
   - [web-ide](https://github.com/zanminkian/web-ide): A shell project publishing to Docker Hub.

## Show your support

Give a ⭐️ if this project helped you!

## License

MIT
