# apt packaging libnvidia nscq

[![License](https://img.shields.io/badge/license-MIT-green.svg)](https://opensource.org/licenses/MIT-license)
[![Contributing](https://img.shields.io/badge/Contributing-Developer%20Certificate%20of%20Origin-violet)](https://developercertificate.org)

## Overview

Packaging templates for `apt` based Linux distros to build libnvidia-nscq packages.

NVIDIA NVSwitch Configuration and Query (NSCQ) library provides a stable driver API used by [DCGM](https://github.com/NVIDIA/DCGM) for monitoring NVSwitch devices.

> _note:_ the version of libnvidia-nscq must match the NVIDIA driver installed.

## Table of Contents

- [Overview](#Overview)
- [Deliverables](#Deliverables)
- [Installation](#Installation)
- [Prerequisites](#Prerequisites)
  * [Clone this git repository](#Clone-this-git-repository)
  * [Install build dependencies](#Install-build-dependencies)
- [Related](#Related)
  * [Fabric Manager](#Fabric-Manager)
  * [NVIDIA driver](#NVIDIA-driver)
- [Contributing](#Contributing)


## Deliverables

This repo contains the template files used to build the following **DEB** packages:


> _note:_ `XXX` is the first `.` delimited field in the driver version, ex: `460` in `460.32.03`

```shell
libnvidia-nscq-XXX
```


## Installation

* **Debian**

  ```shell
  apt-get install libnvidia-nscq-XXX
  ```

* **Ubuntu**

  ```shell
  apt-get install libnvidia-nscq-XXX
  ```


## Prerequisites

### Clone this git repository:

Supported branches: `main`

```shell
git clone https://github.com/NVIDIA/apt-packaging-libnvidia-nscq
```

### Download a NSCQ library tarball:

* TBD

  *ex:* libnvidia-nscq-460.32.03.tar.gz

### Install build dependencies
> *note:* these are only needed for building not installation

```shell
# Packaging
apt-get install debhelper devscripts dpkg-dev
```

## Related

### Fabric Manager

- fabricmanager
  * [https://github.com/NVIDIA/apt-packaging-fabric-manager](https://github.com/NVIDIA/apt-packaging-fabric-manager)

### NVIDIA driver

- nvidia-driver
  * [https://github.com/NVIDIA/ubuntu-packaging-nvidia-driver](https://github.com/NVIDIA/ubuntu-packaging-nvidia-driver)


## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md)
