# pallet-example-overlays
A Forklift pallet demonstrating the use of Forklift to manage system files

## Introduction

pallet-example-overlays is a [Forklift](https://github.com/PlanktoScope/forklift) pallet
specifying an example set of Forklift packages and package deployments for a simple demonstration of
file exports of:

- systemd units to overlay into `/etc`
- config files to overlay into `/etc`
- binaries to overlay into `/usr/bin`

For simplicity, this pallet is structured as a monorepo in which all packages to be deployed are
also defined by the pallet (because this Git repo is declared as both a Forklift pallet and a
Forklift package repository).

## Usage

This pallet is meant to be used together with
[ethanjli/rpi-forklift-demo](https://github.com/ethanjli/rpi-forklift-demo)
as the host OS; for a step-by-step demonstration of the usage of this pallet, follow the
[usage guide of ethanjli/rpi-forklift-demo](https://github.com/ethanjli/rpi-forklift-demo/?tab=readme-ov-file#usage).

If you don't have a Raspberry Pi available to follow the usage guide at ethanjli/rpi-forklift-demo,
the below information may be useful for using this pallet on other systems.

### Prerequisites

You will need to set up the [`forklift`](https://github.com/PlanktoScope/forklift) tool on
your computer. Setup instructions are available
[here](https://github.com/PlanktoScope/forklift?tab=readme-ov-file#downloadinstall-forklift). Note
that currently `forklift` is only tested for Linux computers.

To fully use this demo, you should use a host OS (such as
[ethanjli/rpi-forklift-demo](https://github.com/ethanjli/rpi-forklift-demo) or
[PlanktoScope OS](https://docs-edge.planktoscope.community/reference/software/architecture/os/))
which overlays the `exports/overlays/etc` and/or `exports/overlays/usr` subdirectories of the next
staged Forklift pallet into `/etc` and/or `/usr`, respectively.

### Deployment

You can clone and stage the latest commit of this Forklift pallet to your computer, by
using the `forklift` tool:
```
forklift plt switch --no-cache-img github.com/forklift-run/pallet-example-overlays@main
```

This pallet will create a new directory in `~/.local/share/forklift/stages`; you can determine the
path of that directory by running `forklift stage locate-bun next`. Inside that directory, the
`exports` subdirectory will be a file tree with various files:

- `overlays/etc/systemd/system/hello-world.service`
- `overlays/etc/systemd/system/multi-user.target.wants/hello-world.service`
- `overlays/usr/bin/crane`

If the host OS overlays the `exports/overlays/etc` subdirectory of the next staged Forklift
pallet into `/etc`, after you reboot you should see that:

- Running the command `sudo systemctl status hello-world.service` produces output showing that
  `hello-world.service` ran and printed a hello-world message.
- The hostname of your system has changed to `hello-hostname`.

If the host OS also overlays the `exports/overlays/usr` subdirectory of the next staged Forklift
pallet into `/usr`, after you reboot you should see that:

- You can now run the `crane` command anywhere. For example, if you run
  `crane export alpine:latest - | tar -tvf - | less`, you can use
  [crane](https://github.com/google/go-containerregistry/blob/main/cmd/crane/README.md)
  to list the files in the `alpine:latest` Docker container image.

### Forking

To make your own copy of this repository for experimentation, you should fork this repository to a
new repository. Then, update the `path` fields of the `forklift-pallet.yml` and
`forklift-repository.yml` files to match the path of your new repository.

## Licensing

Forklift packages deployed by this pallet have their own software licenses, as specified in the
definitions of those packages. Any source code provided with this Forklift pallet is covered by the
following information, except where otherwise indicated:

Copyright Ethan Li and Forklift project contributors

SPDX-License-Identifier: Apache-2.0 OR BlueOak-1.0.0

You can use the source code provided here either under the
[Apache 2.0 License](https://www.apache.org/licenses/LICENSE-2.0)
or under the [Blue Oak Model License 1.0.0](https://blueoakcouncil.org/license/1.0.0);
you get to decide. We are making the software available under the Apache license because it's
[OSI-approved](https://writing.kemitchell.com/2019/05/05/Rely-on-OSI.html),
but we like the Blue Oak Model License more because it's easier to read and understand.
