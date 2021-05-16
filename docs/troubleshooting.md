# Troubleshooting

## Development

### Restart buildkit

Often times when coming back to a docker build, the buildkit container needs to
be restarted for some reason.

```bash
------
 > [base 4/4] RUN   echo "**** install packages ****" &&   apk add --no-cache     wget=1.21.1-r1     gzip=1.10-r1:
#9 0.139 container_linux.go:367: starting container process caused: process_linux.go:340: applying cgroup configuration for process caused: mkdir /sys/fs/cgroup/blkio/buildkit: no such file or directory
------
```

To fix this, run the following command to get the container name and then
manually restart the container.

```bash
$ docker ps
CONTAINER ID        IMAGE                           ...  NAMES
75316abfd2e3        moby/buildkit:buildx-stable-1   ...  buildx_buildkit_mybuilder0
$ docker restart buildx_buildkit_mybuilder0
```

### Alpine Packages

Hadolint recommends that the package version be pinned ([DL3018]). However,
Alpine repositories don't keep older version of packages and so building older
versions of Docker images may give errors. To solve these errors, the package
versions need to be updated.

There are multiple ways to get the package version.

1. Use the [online database] to lookup the package version.
2. Use the error output to get the latest version of the package.

```bash
#42 0.206 **** install packages ****
#42 0.210 fetch http://dl-cdn.alpinelinux.org/alpine/v3.12/main/x86_64/APKINDEX.tar.gz
#42 0.332 fetch http://dl-cdn.alpinelinux.org/alpine/v3.12/community/x86_64/APKINDEX.tar.gz
#42 0.442 ERROR: unsatisfiable constraints:
#42 0.458   mysql-client-10.4.17-r1:
#42 0.458     breaks: world[mysql-client=10.4.15-r0]
```

In the above example, the `mysql-client` package version needs to be set to
`10.4.17-r1`.

3. Launch a shell of the Alpine base image and get the policy of the package
you're trying to install.

```bash
$ docker run --rm -it alpine:3.13.1 /bin/ash
$ apk policy <package name>
```

4. Alternatively, you can get the package version directly.

```bash
$ docker run --rm -it alpine:3.13.1 /bin/ash -c "apk update && apk policy <package name>"
...
<package name> policy:
  1.21.1-r1:
...
```

Make sure `alpine:3.13.1` is replaced with the base image version that you're
using and that `<package name>` is replaced with the name of the package you're
trying to install.

## Publishing

If you receive an unauthorized error when trying to publish the image using the
`ci` github action, it's most likely because you either have invalid
credentials, or with respect to quay.io, the repository hasn't been created or
the robot account hasn't been given write access. See
[Publishing#Quay.io](./Publishing#quayio).

```bash
Unauthorized 401
```

[online database]: https://pkgs.alpinelinux.org/packages
[DL3018]: https://github.com/hadolint/hadolint/wiki/DL3018
