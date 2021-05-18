# Goss

[Goss] is used by `ci.yaml` Github Action to validate that the Docker image is
working.

!!! Note
    The goss test will be skipped if the goss.yaml file is not present.

## TL;DR

```shell
# Edit the goss.yaml file
... hack, hack, hack ...
# Test the goss file.
task goss:run
```

## Installation

```shell
# Install latest version to /usr/local/bin
curl -fsSL https://goss.rocks/install | sh

# Install dgoss
curl -L https://raw.githubusercontent.com/aelsabbahy/goss/master/extras/dgoss/dgoss -o /usr/local/bin/dgoss
chmod +rx /usr/local/bin/dgoss
```

[Goss]: https://github.com/aelsabbahy/goss
