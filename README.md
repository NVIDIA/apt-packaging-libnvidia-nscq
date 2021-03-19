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
- [Build-Manually](#Build-Manually)
- [Related](#Related)
  * [Fabric Manager](#Fabric-Manager)
  * [NVIDIA driver](#NVIDIA-driver)
- [See also](#See-also)
  * [RPM](#RPM)
- [Contributing](#Contributing)


## Deliverables

This repo contains the template files used to build the following **DEB** packages:


> _note:_ `XXX` is the first `.` delimited field in the driver version, ex: `460` in `460.32.03`

```shell
- libnvidia-nscq-XXX
> ex: libnvidia-nscq-460_460.32.03-1_amd64.deb
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

### Download a NSCQ tarball:

* https://developer.download.nvidia.com/compute/cuda/redist/libnvidia_nscq/

  *ex:* libnvidia_nscq-linux-x86_64-460.32.03.tar.gz

### Install build dependencies
> *note:* these are only needed for building not installation

```shell
# objdump
apt-get install binutils
# Packaging
apt-get install debhelper devscripts dpkg-dev
```


## Building Manually

### Parse JSON to retrieve download URL
```shell
baseURL="https://developer.download.nvidia.com/compute/cuda/redist"
curl -s $baseURL/redistrib_460.32.03.json | \
jq -r '."libnvidia_nscq" | ."460.32.03" | ."linux-x86_64"' | \
sed "s|^|$baseURL/|"
```

### Create a temp directory
```shell
cd apt-packaging-libnvidia-nscq
mkdir build
rsync -a debian build/
```

### Extract tarball
```shell
tar -C build/ -xf ../libnvidia_nscq*.tar.gz
cd build
mv libnvidia_nscq/* $PWD
rmdir libnvidia_nscq
```

### Check API version
```shell
find -type l -name "*.so.*" | sort -uVr | awk -F ".so." '{print $2}' | awk NR==1
> 1.1
```

### Check SONAME
```shell
objdump -p libnvidia-nscq.so.[4-9][0-9][0-9]* | grep SONAME | awk -F ".so." '{print $2}'
> 1
```

### Fill variables
```shell
make -f debian/rules fill_templates VERSION=460.32.03 BRANCH=460 DEB_HOST_ARCH=amd64
```
> _note:_ branch is the first `.` delimited field in the driver version, ex: `460` in `460.32.03`

### Generate .deb packages
```shell
DEB_BUILD_OPTIONS=nostrip DEB_HOST_ARCH=amd64 \
dpkg-buildpackage -b
cd ..
ls *.deb
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
