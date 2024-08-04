# Outline Server for ARM

This is a fork of the [Outline Server](https://github.com/Jigsaw-Code/outline-server) created to provide a Docker image for ARM64/aarch64 architecture, answering the question of how to run Outline Server on Raspberry Pi, Odroid, or any other ARM64 device.

For some reason, [the official Outline Server Docker image](https://quay.io/repository/outline/shadowbox?tab=tags) is only available for x86_64, even though Outline Server build process [has been upgraded to support aarch64/arm64](https://github.com/Jigsaw-Code/outline-server/pull/1399).

```{tip}
For all intents and purposes, I've no affiliation with Jigsaw or Outline. I don't assume any liability for the use of this image, it's produced "as is" and so on. All right reserved to Jigsaw and Outline's team.
```

## Installation

Instead of the default installation command, run:

```bash
sudo bash -c "SB_IMAGE=mneveroff/outline-server-arm:latest $(wget -qO- https://raw.githubusercontent.com/MNeverOff/outline-server-arm/master/src/server_manager/install_scripts/install_server.sh)"
```

You should see Watchtower and Outline Server containers running on your machine:

## Build from source

If you aren't trusting of 3rd party Docker Hub images and shell scripts hosted on the internet (as you should be), or if I've fallen behind in maintaining this, you can build it from source:

```bash
task shadowbox:docker:build TARGET_ARCH=arm64 VERSION=1.9.0 IMAGE_NAME=mneveroff/outline-server-arm
```

Above, replace the `VERSION` and `IMAGE_NAME` accordingly, and push to Docker Hub if convenient.

Within the [install_script.sh](src/server_manager/install_scripts/install_server.sh) script, replace:

```bash
  if [[ "${MACHINE_TYPE}" != "x86_64" ]]; then
    log_error "Unsupported machine type: ${MACHINE_TYPE}. Please run this script on a x86_64 machine"
    exit 1
  fi
```

with

```bash
  if [[ "${MACHINE_TYPE}" != "x86_64" && "${MACHINE_TYPE}" != "aarch64" ]]; then
    log_error "Unsupported machine type: ${MACHINE_TYPE}. Please run this script on an x86_64 or aarch64 machine"
    exit 1
  fi
```

Or comment it out entirely, your choice.
