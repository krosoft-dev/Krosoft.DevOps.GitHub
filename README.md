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

Set `deploy-pages` to build a static site and publish it to GitHub Pages once
the package is live. The build runs `pages-build-script` (default
`storybook:build`) and deploys `pages-path` (default `storybook-static`).

A caller that enables it must grant the Pages permissions on the calling job:

```yaml
jobs:
  publish:
    permissions:
      contents: read
      pages: write
      id-token: write
    uses: krosoft-dev/Krosoft.DevOps.GitHub/.github/workflows/npm-publish.yml@main
    secrets: inherit
    with:
      deploy-pages: true
```

Callers that leave `deploy-pages` off need nothing beyond `id-token: write`.

The Pages job deliberately declares no `permissions` of its own, so it inherits
whatever the caller granted. A called workflow can only narrow its caller's
permissions, never widen them, and a job asking for more than it was given fails
the whole run at startup — before any `if:` condition is evaluated. Pinning
`pages: write` on the job would therefore break every publisher that does not
deploy Pages, which is exactly what happened until this was fixed.
