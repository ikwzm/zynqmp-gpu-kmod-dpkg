zynqmp gpu (ARM Mali-400) kernel module debian package
====================================================================================

Overview
------------------------------------------------------------------------------------

### Introduction

This is a repository for making zynqmp gpu kernel module debian package.

### Note

This repository only provides script files for creating Debian Packages. Does not include generated Debian Package and kernel module source code. Download the kernel module source code from the ARM web page.

Build Debian Package
------------------------------------------------------------------------------------

### Download this repository

```console
shell$ git clone --recursive --depth=1 -b v0.1.1 git://github.com/ikwzm/zynqmp-gpu-kmod-dpkg
shell$ cd zynqmp-gpu-kmod-dpkg
```

### Download Open Source Mali Utgard GPU Kernel Drivers

#### Download from Web page

https://developer.arm.com/tools-and-software/graphics-and-gaming/mali-drivers/utgard-kernel/

download DX910-SW-99002-r8p0-01rel0.tgz

#### Download with wget

```console
shell$ wget https://developer.arm.com/-/media/Files/downloads/mali-drivers/kernel/mali-utgard-gpu/DX910-SW-99002-r8p0-01rel0.tgz
--2019-12-08 13:55:32--  https://developer.arm.com/-/media/Files/downloads/mali-drivers/kernel/mali-utgard-gpu/DX910-SW-99002-r8p0-01rel0.tgz
Resolving developer.arm.com (developer.arm.com)... 184.26.212.16
Connecting to developer.arm.com (developer.arm.com)|184.26.212.16|:443... connected.
HTTP request sent, awaiting response... 302 Moved Temporarily
Location: https://armkeil.blob.core.windows.net/developer/Files/downloads/mali-drivers/kernel/mali-utgard-gpu/DX910-SW-99002-r8p0-01rel0.tgz [following]
--2019-12-08 13:55:33--  https://armkeil.blob.core.windows.net/developer/Files/downloads/mali-drivers/kernel/mali-utgard-gpu/DX910-SW-99002-r8p0-01rel0.tgz
Resolving armkeil.blob.core.windows.net (armkeil.blob.core.windows.net)... 52.239.137.100
Connecting to armkeil.blob.core.windows.net (armkeil.blob.core.windows.net)|52.239.137.100|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 350213 (342K) [application/octet-stream]
Saving to: ‘DX910-SW-99002-r8p0-01rel0.tgz’

DX910-SW-99002-r8p0-01rel0 100%[========================================>] 342.00K   490KB/s    in 0.7s    

2019-12-08 13:55:35 (490 KB/s) - ‘DX910-SW-99002-r8p0-01rel0.tgz’ saved [350213/350213]
```

#### Extract DX910-SW-99002-r8p0-01rel0.tgz

```console
shell$ tar xfz DX910-SW-99002-r8p0-01rel0.tgz 
```

### Download meta-xilinx

```console
shell$ git clone https://github.com/Xilinx/meta-xilinx.git
Cloning into 'meta-xilinx'...
remote: Enumerating objects: 840, done.        
remote: Counting objects: 100% (840/840), done.        
remote: Compressing objects: 100% (395/395), done.        
remote: Total 11356 (delta 474), reused 732 (delta 404), pack-reused 10516        
Receiving objects: 100% (11356/11356), 8.76 MiB | 6.08 MiB/s, done.
Resolving deltas: 100% (6244/6244), done.
```

### Patch to DX910-SW-99002-r8p0-01rel0

```console
shell$ for file in `\find meta-xilinx/meta-xilinx-bsp/recipes-graphics/mali/kernel-module-mali -maxdepth 1 -type f | sort`; do patch -d DX910-SW-99002-r8p0-01rel0/driver/src/devicedrv/mali/ -p1 < $file ; done
patching file Makefile
patching file platform/arm/arm.c
patching file linux/mali_linux_trace.h
patching file platform/arm/arm.c
patching file linux/mali_kernel_linux.c
patching file platform/arm/arm.c
patching file linux/mali_memory_os_alloc.c
patching file linux/mali_osk_notification.c
patching file linux/mali_internal_sync.c
patching file linux/mali_internal_sync.h
patching file linux/mali_memory_swap_alloc.c
patching file common/mali_pm.c
patching file linux/mali_kernel_linux.c
patching file linux/mali_memory_os_alloc.c
patching file linux/mali_memory_secure.c
patching file common/mali_control_timer.c
patching file common/mali_group.c
patching file common/mali_osk.h
patching file linux/mali_osk_timers.c
```

### Cross Compile

#### Parameters

| Parameter Name | Description              | Default Value                                                    |
|----------------|--------------------------|------------------------------------------------------------------|
| KERNEL_RELEASE | Kernel Release Name      | $(shell uname -r)                                                |
| ARCH           | Architecture Name        | $(shell uname -m \| sed -e s/arm.\*/arm/ -e s/aarch64.\*/arm64/) |
| DEB_ARCH       | Debian Architecture Name | $(shell dpkg --print-architecture)                               |
| KERNEL_SRC_DIR | Kernel Source Directory  | /lib/modules/$(kernel_release)/build                             |


```console
shell$ sudo debian/rules ARCH=arm64 DEB_ARCH=arm64 KERNEL_RELEASE=4.19.0-xlnx-v2019.1-zynqmp-fpga KERNEL_SRC_DIR=~/work/ZynqMP-FPGA-Linux/linux-xlnx-v2019.1-zynqmp-fpga binary
    :
    :
    :
shell$ file ../zynqmp-gpu-4.19.0-xlnx-v2019.1-zynqmp-fpga_0.1.1-0_arm64.deb 
../zynqmp-gpu-4.19.0-xlnx-v2019.1-zynqmp-fpga_0.1.1-0_arm64.deb: Debian binary package (format 2.0)
```

### Self Compile

```console
shell$ sudo debian/rules binary
    :
    :
    :
shell$ file ../zynqmp-gpu-4.19.0-xlnx-v2019.1-zynqmp-fpga_0.1.1-0_arm64.deb 
../zynqmp-gpu-4.19.0-xlnx-v2019.1-zynqmp-fpga_0.1.1-0_arm64.deb: Debian binary package (format 2.0)
```

