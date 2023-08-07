
Introduction
============
This repo can be used to build a 2.6.31.x Linux kernel for the MIPS Malta board usable with QEMU.
It's forked from the firmadyne repo but it _does not_ include the `firmadyne` module, though many of
the other customizations are still included (ssh, other services). No filesystem image is provided.

Changing Kernel Versions
============

The kernel version to be build can be updated by modifying the `Makefile` at the root of the repo at
changing these lines at the top of the file:

```
# version - 2.6.31.6
VERSION = 2
PATCHLEVEL = 6
SUBLEVEL = 31
EXTRAVERSION = .6
```

Usage
=====

Since MIPS systems can be either big-endian or little-endian, the kernel
can be compiled for both endianness.

Little-Endian (MIPSEL)
----------------------

Create the kernel build output directory:

`mkdir -p build/mipsel`

Copy the configuration file into the build directory:

`cp config.mipsel build/mipsel/.config`

Assuming that the appropriate cross-compiler is installed in `/opt/cross/mipsel-linux-musl`, execute:

`make ARCH=mips CROSS_COMPILE=/opt/cross/mipsel-linux-musl/bin/mipsel-linux-musl- O=./build/mipsel -j8`

Alternative command for Ubuntu/Debian systems with the `gcc-mipsel-linux-gnu` metapackage installed:

`make ARCH=mips CROSS_COMPILE=mipsel-linux-gnu- O=./build/mipsel -j8`

The output kernel image will be generated at the following location:

`build/mipsel/vmlinux`

Big-Endian (MIPSEB)
-------------------

Create the kernel build output directory:

`mkdir -p build/mipseb`

Copy the configuration file into the build directory:

`cp config.mipseb build/mipseb/.config`

Assuming that the appropriate cross-compiler is installed in `/opt/cross/mipseb-linux-musl`, execute:

`make ARCH=mips CROSS_COMPILE=/opt/cross/mipseb-linux-musl/bin/mipseb-linux-musl- O=./build/mipseb -j8`

Alternative command for Ubuntu/Debian systems with the `gcc-mips-linux-gnu` metapackage installed:

`make ARCH=mips CROSS_COMPILE=mips-linux-gnu- O=./build/mipseb -j8`

The output kernel image will be generated at the following location:

`build/mipseb/vmlinux`

Notes
=====

This instrumented MIPS kernel is built for the `ARCH_MALTA`
[MIPS Malta](http://wiki.qemu.org/download/qemu-doc.html#MIPS-System-emulator)
target with a 24kf processor. As a result, hardware support on this
emulated target is limited to peripherals that are both available on the
emulated target and supported by QEMU. Since an emulated PCI bus is available
and supported, this allows additional ethernet devices (e.g. `rtl8139`, 
`smc91c111`, `pcnet32`, etc.) to be attached to the virtualized system. 
Emulated hard drives can be attached using the IDE block device interface.

As future work, it may be useful to switch to
[VirtIO](http://wiki.libvirt.org/page/Virtio) on MIPS, since support has been
recently merged into the Linux kernel. However, this would require a kernel
upgrade. Additionally, it may be useful to add support for MIPS64 systems,
although these do not appear to be prevalent. Nevertheless, at the time, we
performed our published experiments over our dataset using this kernel for
MIPS systems.

