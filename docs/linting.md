# Linting

My Docker repositories use [yamllint] and [hadolint] to lint the repositories.

```shell
# Use the Makefile to lint the repository.
make lint
```

## Yamllint

```shell
# Manually lint
yamllint .
```

## Hadolint
```shell
# Manually lint
hadolint Dockerfile
```

### Pin Package Versions ([DL3008])

1. Copy the packages list and name of the base image from the `Dockerfile` to
variables `PACKAGES` and `BASE` respectively in `make.env/task.env`.
2. Run `make packages/task packages` to list the package versions.

```shell
task packages-alpine
...
ca-certificates policy:
  20191127-r5:
    lib/apk/db/installed
    http://dl-cdn.alpinelinux.org/alpine/v3.13/main
...
```
3. Copy the package versions from `stdout` over to the `Dockerfile`.

Similar steps may be taken to pin `pip` ([DL3013]) and `npm` ([DL3016])
packages.

Alternatively, a `shell` may be launched of the build image and manually run
`apk policy <package>` or `apt-update && apt-cache policy <package>` to get
the package versions.

## Pre-commit hook

If you want to automatically generate `README.md` files with a pre-commit hook,
make sure you [install the pre-commit binary], and add a
`.pre-commit-config.yaml` to your project. Then run:

```shell
pre-commit install
pre-commit install-hooks
```

Currently, this only works on `amd64` systems.

## Github Action
The [lint github action] is also used to lint the repo upon commit.

[yamllint]: https://github.com/adrienverge/yamllint
[hadolint]: https://github.com/hadolint/hadolint
[lint github action]: https://github.com/nicholaswilde/docker-template/blob/main/.github/workflows/lint.yaml
[install the pre-commit binary]: https://pre-commit.com/#install
[DL3016]: https://github.com/hadolint/hadolint/wiki/DL3016
[DL3013]: https://github.com/hadolint/hadolint/wiki/DL3013
[DL3008]: https://github.com/hadolint/hadolint/wiki/DL3008
