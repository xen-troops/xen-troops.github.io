# Base xen-troops products

Main development platform for Xen Troops is Renesas R-Car gen3

## Development: `prod-devel`

This configurable product is serving as a base for all custom products used during the course of development.
* dom0 with generic armv8 Linux
* domd based on Renesas Yocto BSP with AGL patches
* doma with Android
* domf with generic armv8 Fusion

`prod-devel` uses **HEAD**s of **'master'** branches of all development repositories

### Prerequisites

* Xen 4.10
* Renesas Yocto BSP 3.7 with IMG DDK 1.9/4813199
* Linux kernel 4.14 across all domains
* AGL 5.0.2
* Android version is 8.1 with IMG DDK 1.9/ED4813199

### Supported boards

* Salvator-X with R-Car M3 ES1.1 / H3 ES2.0 / H3 ES3.0-8GB
* Salvator-XS with R-Car H3 ES2.0

## Internal demo: `prod-demo`

Configurable product that has the same system layout like `prod-devel` and used for demos performed by Xen Troops team.

The `prod-demo` product uses some **'release' tags** of **'master'** branches of all development repositories

List of per-component `prod-demo` tags will be defined later

## Renesas test system: `prod-gen3-test`

SoM testing product with following layout:
* dom0 with generic armv8 Linux
* domd based on Renesas Yocto BSP
* domu based on Renesas Yocto BSP

### Prerequisites

* Xen 4.10
* Renesas Yocto BSP 2.23.1 with IMG DDK 1.9/4908383
* Linux kernel 4.9 across all domains

### Supported boards

* Salvator-X with R-Car M3 ES1.1 / H3 ES2.0
* Salvator-XS with R-Car M3N ES1.0

List of per-component `prod-gen3-test` tags will be defined later

## Xen community test system: `prod-xentest`

### Supported boards

* Starter Kit Premium with R-Car H3 ES2.0
* ADAS Starter Kit with R-Car H2
