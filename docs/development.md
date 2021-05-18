# Development

## Setup

My docker images are typically linux multi-architecture (armv7, arm64, & amd64)
because I run a RPi cluster. I use
[buildx](https://docs.docker.com/engine/reference/commandline/buildx/) to build
my multi-architecture images.

Check that you can build the following:

```bash
docker buildx ls
NAME/NODE    DRIVER/ENDPOINT             STATUS  PLATFORMS
mybuilder *  docker-container
  mybuilder0 unix:///var/run/docker.sock running linux/amd64, linux/arm64, linux/arm/v7
```

If you are having trouble building arm images on a x86 machine, see
[this blog post](https://www.docker.com/blog/getting-started-with-docker-for-arm-on-linux/).

Also checkout
[qemu-user-static](https://github.com/multiarch/qemu-user-static#getting-started).

```bash
docker run --rm --privileged multiarch/qemu-user-static:register --reset
```

## Task

The docker repos use [task](./task.md) to automate the development processes.

## TL;DR

### Create Image

```shell
# Create a new repo from the template repo.
https://github.com/nicholaswilde/docker-template/generate
# Manually add app version to the VERSION file. Make sure a new line character does not exist at the end of the file.
printf <version number> > VERSION
# Update task.env with all appropriate values.
... hack, hack, hack ...
# Get the checksum of the release or source package.
task chk:print
# Export the checksum of the release or source package to the CHECKSUM file.
task chk:export
# Create the Dockerfile
... hack, hack, hack ...
# Build the image
task build
# Load the image
task load
# Run the image
task run
# Update goss.yaml
... hack, hack, hack ...
# Test the image with GOSS
task goss:run
```

### Update Image

```shell
# Print the latest app version and confirm that the correct version is pulled.
task version:print
# Export the version to the VERSION file
task version:export
# Get the checksum of the release or source package.
task chk:print
# Export the checksum of the release or source package to the CHECKSUM file.
task chk:export
# Increment the LS value
task ls:increment
# Update the Dockerfile
... hack, hack, hack ...
# Build the image
task build
# Load the image
task load
# Run the image
task run
# Test the image with GOSS
task goss:run
```

## Directory Layout

```
docker-appname
├── .github/workflows
│           └── ci.yaml
├── CHECKSUM
├── docker-compose.yaml
├── Dockerfile
├── goss.yaml
├── LS
├── task.env
└── VERSION
```

## Files

The `CHECKSUM`, `LS`, and `VERSION` files are all used to pass variables to the
`ci.yaml` Github Action.

!!! Note
    For the commands below, `printf` should be used rather than `echo` so that
    a newline character is not added to the end of the files. This needs to be
    done so in order for the ci.yaml github action can read the values into
    variables correctly.

### CHECKSUM

Used to pass in the sha256 checksum of the downloaded file.

```shell
# Manual
printf "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855" > CHECKSUM
# Task
task chk:export
```

### Dockerfile

The Dockerfile, of course!

### goss.yaml

The [Goss](./goss.md) config file used by the `ci.yaml` Github Action.

### LS

Used to pass in the image version.

!!! Note
    My Docker images are based off of [LinuxServer.io's]. LS, I _believe_ stands
    for LinuxServer.

```shell
# Manual
printf "2" > LS
# Task
task ls:increment
```

### task.env

Used to pass other variables into Task.

### VERSION

Used to pass in the app version.

```shell
# Manual
printf "0.1.0" > VERSION
# Task
task version:export
```

## Troubleshooting

See [Troubleshooting](./troubleshooting.md)

[LinuxServer.io's]: https://github.com/linuxserver/
