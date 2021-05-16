# Troubleshooting

## Setup

1. Browse to `.github/workflows/ci.yaml` file.
2. Take note of the required github secrets. E.g. `DOCKERHUB_TOKEN`.
3. Add the [repo secrets].

### Quay.io

For the Quay.io registry, you'll need to manually create an image repository.
It does not create one upon first push like Docker Hub.

1. [Create a quay.io image repository].
2. Give `write` access to a [robot] for the newly created image repository.

## Publishing

Publishing images is done through the [ci github action]. The action is
manually triggered in order to be able to set the `VERSION` and `LS` variables.
The github action also creates a release.

The name of the image is taken from the repository name after `docker-`. So,
repo `docker-etherpad` would have an image name of `etherpad`.

Both the current version tag and the latest tag are released.

### Github Repository

1. Browse to the [Actions tab].
2. Click on the `ci` workflow.
3. [Manually trigger] the workflow using the app version and image (ls)
version.

### Github Packages

By default, a `ghcr.io` package is published as private and so after the
initial release to the `ghcr.io` registry, the package will need to be
converted to a public package if you want anyone to use it.

1. See [Configuring visibility of container images for your personal account][1].
2. You may also set connect your repository to your container image/package.
See [Connecting a repository to a container image].

## Troubleshooting

See [Troubleshooting](./troubleshooting)

[repo secrets]: https://docs.github.com/en/actions/reference/encrypted-secrets#creating-encrypted-secrets-for-a-repository
[Create a quay.io image repository]: https://docs.quay.io/guides/create-repo.html
[robot]: https://docs.quay.io/glossary/robot-accounts.html
[ci github action]: https://github.com/nicholaswilde/docker-template/blob/main/.github/workflows/ci.yaml
[Actions tab]: https://github.com/nicholaswilde/docker-template/actions
[Manually trigger]: https://github.blog/changelog/2020-07-06-github-actions-manual-triggers-with-workflow_dispatch/
[Connecting a repository to a container image]: https://docs.github.com/en/packages/guides/connecting-a-repository-to-a-container-image
[1]: https://docs.github.com/en/packages/guides/configuring-access-control-and-visibility-for-container-images#configuring-visibility-of-container-images-for-your-personal-account
