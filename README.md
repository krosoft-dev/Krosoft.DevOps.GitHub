# Krosoft.DevOps.GitHub

Reusable GitHub Actions workflows for npm packages.

## Workflows

### `npm-build.yml`

Installs dependencies and builds the package.

The build step runs the `build` npm script by default. Override `build-script`
to run another one — for instance `storybook:build` to build a Storybook.

### `npm-publish.yml`

Builds, versions and publishes the package to npm. The version is read from the
registry and bumped by one patch — it is never committed back, so `package.json`
stays at its repository value.

Set `deploy-pages` to publish a static site to GitHub Pages once the package is
out. The site is built *after* the publish, so it can display the version that
has just been released:

```yaml
jobs:
  publish:
    permissions:
      contents: read
      id-token: write
      pages: write
    uses: krosoft-dev/Krosoft.DevOps.GitHub/.github/workflows/npm-publish.yml@main
    with:
      deploy-pages: true
      pages-build-script: storybook:build
      pages-path: storybook-static
    secrets: inherit
```

The calling job must grant `pages: write` and `contents: read`: a reusable
workflow cannot request more than its caller gives it. The repository also needs
Pages enabled with **GitHub Actions** as its source — the workflow attempts it on
the first run.
