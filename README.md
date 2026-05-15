# github-actions

A collection of reusable GitHub Actions for automating your workflows.

## All-in-One Action

Use only one workflow file to automate your entire test and release process.

### Key Features

- 🦋 **Powered by Changesets**: Seamless use this action if the project uses [changesets](https://github.com/changesets/changesets).
- 🛡️ **NPM Provenance**: Ensures secure and verifiable releases.
- 📦 **Package Manager Support**: Supports `npm`, `pnpm`, and `yarn`.
- 🛠️ **Customizable Scripts**: Allows defining custom scripts for testing, building, and publishing in your `package.json`.
- 🏗️ **Cross-Platform Build**: Supports running the build job on multiple operating systems via the `builds-on` parameter.
- 🔐 **Secret Management**: Simplifies secret management through environment variables.

### Setup Guide

#### 1. **Configure Node.js Version and Define Publishing Scripts**

Specify a compatible Node.js version by `devEngines` and define `test`, `build`, and `release` scripts in your `package.json`. Example:

```json
{
  "devEngines": {
    "runtime": {
      "name": "node",
      "version": ">=22.0.0"
    }
  },
  "scripts": {
    "build": "tsc -p .", // Replace with your actual build script
    "test": "echo 'pass'", // Replace with your actual test script
    "release": "pnpm -r publish" // Replace with your actual publish script
  }
}
```

#### 2. **Set Up Secrets (Optional)**

Create a repository secret named `TOKENS` in your GitHub repository settings under `Settings > Secrets > Actions` if you need additional authentication tokens for publishing. This should be a JSON string where each key-value pair becomes an environment variable during the release process.

Example:

```json
{
  "NPM_TOKEN": "your_npm_token",
  "DOCKERHUB_TOKEN": "your_dockerhub_token"
}
```

#### 3. **Create Workflow File**

Add a workflow file in `.github/workflows/`. Example configuration:

```yaml
name: CI

on:
  - push
  - pull_request

jobs:
  ci:
    permissions:
      contents: write
      id-token: write
      pull-requests: write
    uses: zanminkian/github-actions/.github/workflows/all-in-one.yml@v3
    with:
      builds-on: '["ubuntu-latest"]' # optional, e.g. '["ubuntu-latest", "windows-latest"]'
      # assets: dist # optional, a directory path to release assets
    secrets:
      TOKENS: ${{ secrets.TOKENS }} # optional
```

- `builds-on` — A JSON array of OS names to run the build job on. If omitted, the build job is skipped entirely. Useful for cross-platform native addons or binaries.
- `assets` — A directory path containing release assets to attach to the GitHub release. When used together with `builds-on`, assets from all build matrix jobs are merged.

#### 4. **Commit and Push**

Commit the workflow file and push it to the default branch. Each push will trigger the test process, while only pushes to the `main` and `prerelease` branch may trigger the release process.

### Troubleshooting

- **Git Status Not Clean**: Ensure all changes are committed before running the workflow.
- **Missing Secrets**: Verify that the `TOKENS` secret is correctly set in your repository settings.
- **Incompatible Node.js Version**: Update the `devEngines.runtime.version` field in your `package.json`.

### Example Projects

- [fenge](https://github.com/zanminkian/fenge): A TypeScript project publishing to npm.
- [@rnm/tscx](https://github.com/rnmjs/tscx): A wrapper around `tsc`.
- [web-ide](https://github.com/zanminkian/web-ide): A project publishing to Docker Hub.

# License

MIT
