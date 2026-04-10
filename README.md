# CICD Utils

A collection of utility scripts for Ohio University's Software Engineering Team's CICD pipeline.

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
