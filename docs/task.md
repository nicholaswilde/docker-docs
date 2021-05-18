# Task

The docker repos use [task](https://github.com/go-task/task/) to automate the
development processes.

## Installation

```
# For Default Installion to ./bin with debug logging
sh -c "$(curl --location https://taskfile.dev/install.sh)" -- -d

# For Installation To /usr/local/bin for userwide access with debug logging
# May require sudo sh
sh -c "$(curl --location https://taskfile.dev/install.sh)" -- -d -b /usr/local/bin
```

## Help

```shell
task
task: Available tasks for this project:
* build:                Build the native Docker image
...
```
