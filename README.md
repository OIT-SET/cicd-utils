# CICD Utils

A collection of utility scripts for Ohio University's Software Engineering Team's CICD pipeline.

## Scripts

### Builder

Runs install and build actions for NestJS-style projects. Supports (`install|build`) actions and build type (`dev|prod`). During installs, it runs `npm ci` with a mounted npm config. For `prod` builds, it runs `npm run build` and then removes source code in `src` and `test`.

Requirements:
- `npm` is available.
- npmrc is mounted at `/kaniko/npm/npmrc` or `/run/secrets/npmrc`.

### Install Dependencies

Downloads pinned release artifacts (`builder`, optional `oracle-setup`, and `j2tmpl`) into `/usr/bin`, makes them executable, and verifies each file with SHA-256 checksums. Supports repo-type specific behavior, including Oracle setup for `nestjs-thick`.

Requirements:
- `wget`, `sha256sum`, and `chmod` are available.
- User has write access to `/usr/bin`.

### Oracle Setup

Finds the Oracle client directory that matches `ORACLE_CLIENT_VERSION`, creates a stable symlink at `/usr/lib/oracle/current`, and installs required runtime libraries. The script fails fast if the version variable is missing or if client discovery is ambiguous.

Requirements:
- `ORACLE_CLIENT_VERSION` environment variable exists.
- Debian/Ubuntu-like base image with `apt-get`.
- User has root privileges.
- Oracle client present under `/usr/lib/oracle`.

## When Updating

For every file that was changed and downloaded via the `install-dependencies` script:
1. Calculate the sha256sum of the file `sha256sum ./scripts/filename`
1. Using the sha256sum, update the corresponding hash var in the `install-dependencies` script.

## Releasing a New Version

1. After your pull request has been merged, create a new release by navigating to the "Releases" section of the repository on GitHub and clicking "Draft a new release".
1. Create a new tag for the release, following the format `vX.Y.Z` (e.g., `v0.0.1`).
1. Set the target as "main".
1. The release title should be the same as the tag (e.g., `v0.0.1`).
1. Click "Generate release notes" to automatically generate the release notes based on the merged pull requests.
1. Attach **all** scripts in the `scripts` directory to the release.
1. Untick the "Set as a pre-release" checkbox.
1. Publish the release by clicking "Publish release".

## Using a New Version

1. After a new release has been published, update the version in *your project*'s Dockerfile to use the new version.
1. Update the hash value for `install-dependencies` to the one provided in the release page.
