# Base xen-troops products

## Development: `prod-devel`

Configurable product with following layout:
* dom0 with generic armv8 Linux
* domd based on Renesas Yocto with AGL patches
* doma with Android
* domf with generic armv8 Fusion

`prod-devel` uses **HEAD**s of **'master'** branches of all development repositories

### Prerequisites

* Main development platform is Renesas R-Car gen3
* Xen 4.10
* Renesas Yocto BSP 3.7 with IMG DDK 1.9/4813199
* Linux kernel 4.14 across all domains
* AGL 5.0.2
* Android version is 8.1 with IMG DDK 1.9/ED4813199

### Supported boards

* Salvator-X with R-Car M3 ES 1.1 / H3 ES2.0 / H3 ES3.0-8GB
* Salvator-XS with R-Car H3 ES3.0

## Internal demo: `prod-demo`

Configurable product that has the same system layout like `prod-devel`.

The `prod-demo` product uses some **'release' tags** of **'master'** branches of all development repositories

List of per-component `prod-demo` tags will be defined later

## Renesas test system: `prod-gen3test`

### Supported boards

* Salvator-XS with R-Car M3N

## Xen community test system: `prod-xentest`

### Supported boards

* Starter Kit Premium with R-Car H3 ES2.0
* ADAS Starter Kit with R-Car H2
