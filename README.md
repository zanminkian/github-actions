# github-actions

A collection of reusable GitHub Actions for automating your workflows.

## Release Action

This action automates the process of releasing your project's distribution to platforms such as npm, Docker Hub, and others. It is built on top of [changesets/action](https://github.com/changesets/action) and includes support for NPM Provenance and multiple package managers.

### Key Features

- 🛡️ **NPM Provenance**: Ensures secure and verifiable releases.
- 📦 **Package Manager Support**: Supports `npm`, `pnpm`, and `yarn`.
- 🛠️ **Customizable Scripts**: Allows defining custom scripts for testing and publishing in your `package.json`.
- 🔐 **Secret Management**: Simplifies secret management through environment variables.

### Setup Guide

#### 1. **Configure Node.js Version and Define Publishing Scripts**

Specify a compatible Node.js version and define necessary scripts in your `package.json`. Example:

```json
{
  "engines": {
    "node": ">=22.0.0"
  },
  "scripts": {
    "test": "echo 'pass'", // Replace with your actual test script
    "release": "pnpm -r publish" // Replace with your actual publish script
  }
}
```

#### 2. **Set Up Secrets**

Create a repository secret named `TOKENS` in your GitHub repository settings under `Settings > Secrets > Actions`. This should be a JSON string where each key-value pair becomes an environment variable during the release process. Example:

```json
{
  "NPM_TOKEN": "your_npm_token",
  "DOCKERHUB_TOKEN": "your_dockerhub_token"
}
```

#### 3. **Create Workflow File**

Add a workflow file in `.github/workflows/`. Example configuration:

```yaml
name: Release

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
    uses: zanminkian/github-actions/.github/workflows/release.yml@v2
    secrets:
      TOKENS: ${{ secrets.TOKENS }}
```

#### 4. **Commit and Push**

Commit the workflow file and push it to the configured branch (`main` or `prerelease`). The workflow will run automatically when changes are pushed to the configured branches.

### Troubleshooting

- **Git Status Not Clean**: Ensure all changes are committed before running the workflow.
- **Missing Secrets**: Verify that the `TOKENS` secret is correctly set in your repository settings.
- **Incompatible Node.js Version**: Update the `engines.node` field in your `package.json`.

### Example Projects

- [fenge](https://github.com/zanminkian/fenge): A TypeScript project publishing to npm.
- [@rnm/tscx](https://github.com/rnmjs/tscx): A wrapper around `tsc`.
- [web-ide](https://github.com/zanminkian/web-ide): A project publishing to Docker Hub.

# License

MIT
