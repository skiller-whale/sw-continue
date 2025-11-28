# Releasing sw-continue

This document explains how to release the sw-continue CLI.

## Release Process

### Create a Release Branch

Create and push a release branch matching the version pattern:

```bash
# For version 1.0.0
git checkout -b v1.0.0

# Make any final changes if needed
git add .
git commit -m "chore: prepare release v1.0.0"

# Push the branch - this triggers the release workflow
git push origin v1.0.0
```

The "GitHub Release" workflow will automatically:

- Detect the branch (pattern: `v[0-9]+.[0-9]+.[0-9]+*`)
- Build both CLI and VS Code extension
- **Create a git tag** `v1.0.0` from the branch
- Create a GitHub release with artifacts
- Generate download URLs
- Auto-detect prereleases (if version contains `alpha`, `beta`, `rc`, `pre`)

**Note**: The workflow creates the tag, so you don't need to manually create one. Just push the branch.

**Download URLs** will follow this pattern:

```
https://github.com/skiller-whale/sw-continue/releases/download/v1.0.0/sw-continue-cli-1.0.0.tar.gz
https://github.com/skiller-whale/sw-continue/releases/download/v1.0.0/sw-continue-vscode-1.0.0.vsix
```

### Alternative: Direct Tag Push

For quick releases, you can also push a tag directly:

```bash
git tag v1.0.0
git push origin v1.0.0
```

The "Tagged Release" workflow will create a release from the tag.

## Using the Released CLI

### From Tarball

```bash
curl -L https://github.com/skiller-whale/sw-continue/releases/download/v1.0.0/sw-continue-cli-1.0.0.tar.gz | tar xz
./dist/cn.js --help
```

### From Zip

```bash
# Download and extract
curl -L https://github.com/skiller-whale/sw-continue/releases/download/v1.0.0/sw-continue-cli-1.0.0.zip -o cli.zip
unzip cli.zip
./dist/cn.js --help
```

### Add to PATH

```bash
# Extract to a location in your PATH
tar xz -C /usr/local/bin --strip-components=1 -f sw-continue-cli-1.0.0.tar.gz
cn --help
```

## Version Format

Use semantic versioning:

- **Stable**: `1.0.0`, `1.2.3`
- **Beta**: `1.0.0-beta.1`, `1.0.0-beta.2`
- **Alpha**: `1.0.0-alpha.1`
- **Release Candidate**: `1.0.0-rc.1`

## Current Workflows

### `github-release.yml` (Recommended)

- **Trigger**: Push to release branch matching `v[0-9]+.[0-9]+.[0-9]+*` (e.g., `v1.0.0`, `v1.0.0-beta.1`)
- **Assets**: CLI tarball + VS Code VSIX
- **Prerelease Detection**: Automatic (checks if version contains alpha/beta/rc/pre)

### `tagged-release.yml` (Alternative)

- **Trigger**: Push any tag matching `v[0-9]+.[0-9]+.[0-9]+*`
- **Assets**: CLI tarball + VS Code VSIX
- **Prerelease Detection**: Automatic (checks if tag contains alpha/beta/rc/pre)

## Removed Workflows

The original Continue workflows (`auto-release.yml`, `beta-release.yml`, `stable-release.yml`) were designed for npm publishing. These have been replaced with simple GitHub-based releases.

If you need npm publishing in the future, those can be re-enabled or updated.
