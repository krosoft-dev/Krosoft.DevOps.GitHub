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

