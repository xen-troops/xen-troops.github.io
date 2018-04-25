# Base xen-troops products

## Development: `prod-devel`
Configurable product that include:
* dom0 with generic armv8 linux
* domd based on Renesas Yocto with AGL patches
* domu with Android
* domu with generic armv8 Fusion

`prod-devel` uses **HEAD**s of **'master'** branches of all development repositories

#### Prerequisites
* Main development platform is Renesas R-Car gen3
* Xen version is 4.10
* Renesas Yocto BSP is 3.7
* Linux kernel version across all domains is 4.14
* AGL version is 5.0.2
* Android version is 8.1
* IMG DDK version is 1.9/ED4813199 (with virtualization, Android and OCL support) :poop:

## Internal demo: `prod-demo`
Configurable product that has the same system layout like `prod-devel`.

The `prod-demo` product uses some **'release' tags** of **'master'** branches of all development repositories
