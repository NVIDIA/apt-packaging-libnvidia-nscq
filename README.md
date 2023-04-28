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
- [Building Manually](#Building-Manually)
- [Related](#Related)
  * [Fabric Manager](#Fabric-Manager)
  * [NVIDIA driver](#NVIDIA-driver)
- [See also](#See-also)
  * [RPM](#RPM)
- [Contributing](#Contributing)


## Deliverables

This repo contains the template files used to build the following **DEB** packages:


> _note:_ `XXX` is the first `.` delimited field in the driver version, ex: `525` in `525.85.12`

```shell
- libnvidia-nscq-XXX
> ex: libnvidia-nscq-525_525.85.12-1_amd64.deb
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

Supported branches as described in the [NVIDIA Datacenter Drivers](https://docs.nvidia.com/datacenter/tesla/drivers/index.html#cuda-drivers) documentation.

```shell
git clone https://github.com/NVIDIA/apt-packaging-libnvidia-nscq
```

### Download a NSCQ tarball:

* https://developer.download.nvidia.com/compute/nvidia-driver/redist/libnvidia_nscq/

  *ex:* libnvidia_nscq-linux-x86_64-525.85.12-archive.tar.xz

### Install build dependencies
> *note:* these are only needed for building not installation

```shell
# objdump
apt-get install binutils
# Packaging
apt-get install debhelper devscripts dpkg-dev
```


## Building Manually

### Download tarball via redistrib JSON
```shell
baseURL="https://developer.download.nvidia.com/compute/nvidia-driver/redist"
downloadURL=$(curl -s $baseURL/redistrib_525.85.12.json | \
jq -r '."libnvidia_nscq" | ."linux-x86_64" | ."relative_path"' | \
sed "s|^|$baseURL/|")
curl -O $downloadURL
```

### Create a temp directory
```shell
cd apt-packaging-libnvidia-nscq
mkdir build
rsync -a debian build/
```

### Download and extract tarball
```shell
tar -C build/ -xf ../libnvidia_nscq*.tar.xz
cd build
mv libnvidia_nscq*/* $PWD
rmdir libnvidia_nscq*
```

### Check API version
```shell
find -type l -name "*.so.*" | sort -uVr | awk -F ".so." '{print $2}' | awk NR==1
> 2.0
```

### Check SONAME
```shell
objdump -p /dev/stdin < $(find -type f -name "libnvidia-nscq.so.*") | \
grep SONAME | awk -F ".so." '{print $2}'
> 2
```

### Fill variables
```shell
make -f debian/rules fill_templates VERSION=525.85.12 BRANCH=525 DEB_HOST_ARCH=amd64
```
> _note:_ branch is the first `.` delimited field in the driver version, ex: `525` in `525.85.12`

### Generate .deb packages
```shell
DEB_BUILD_OPTIONS=nostrip DEB_HOST_ARCH=amd64 \
dpkg-buildpackage -b -aamd64
cd ..
ls *.deb
```
> _note:_ for SBSA (arm64 server), pass `DEB_HOST_ARCH=arm64` and `-aarm64`


## Related

### Fabric Manager

- fabricmanager
  * [https://github.com/NVIDIA/apt-packaging-fabric-manager](https://github.com/NVIDIA/apt-packaging-fabric-manager)

### NVIDIA driver

- nvidia-driver
  * [https://github.com/NVIDIA/ubuntu-packaging-nvidia-driver](https://github.com/NVIDIA/ubuntu-packaging-nvidia-driver)


## See also

### RPM

  * [https://github.com/NVIDIA/yum-packaging-libnvidia-nscq](https://github.com/NVIDIA/yum-packaging-libnvidia-nscq)


## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md)
