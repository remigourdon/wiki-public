# Buildroot

Great material available from [Bootlin](https://bootlin.com/doc/training/buildroot/) (check regularly for updates).

Also a [great article here](https://www.blaess.fr/christophe/articles/creer-un-systeme-complet-avec-buildroot/) and a more recent version [here](https://www.blaess.fr/christophe/buildroot-lab/index.html).

One LTS release every year in February and new versions every 3 months.

Install `graphviz` for graph capabilities.

## Embedded Linux Work

+ BSP (Board Support Package): porting the bootloader and Linux kernel, developing Linux device drivers
+ System integration: assembling and configuring user space componenents, upgrade and recovery mechanisms, etc
+ Application development: writing company-specific applications and libraries

## Build System

+ Takes in open-source components (from http, ftp, git, etc) and in-house components
+ Takes in configuration files
+ Produces a root filesystem image, a kernel image, a bootloader image and a toolchain

Advantages:
+ Builds from source which gives a lot of flexibility
+ Cross-compiles which means we can leverage fast build machines
+ Available recipes to build components

## Directory Structure

+ `output/images`: root filesystem image(s), kernel image, device tree blob(s), booloader image(s)
    + can be customized with `O=/path/to/directory`
+ `.config`: configuration file
    + located in out of tree build directory if set

## defconfig

Stores the values from the configuration that are non-default.
The defconfig for the default buildroot configuration is thus empty!

A defconfig is loaded with `make <foo>_defconfig` and overrides the current `.config`.

A defconfig is created with `make savedefconfig`.

## Target Packages

They can install files in 3 different locations:
+ `$(TARGET_DIR)` is the target root filesystem
    + by using `$(<pkg>_INSTALL_TARGET)` (defaults to **YES**)
    + will call `$(<pkg>_INSTALL_TARGET_CMDS)`
+ `$(STAGING_DIR)` is the compiler sysroot
    + by using `$(<pkg>_INSTALL_STAGING)` (defaults to **NO**)
    + will call `$(<pkg>_INSTALL_STAGING_CMDS)`
+ `$(BINARIES_DIR)` is where the final images are located
    + by using `$(<pkg>_INSTALL_IMAGES)` (defaults to **NO**)
    + will call `$(<pkg>_INSTALL_IMAGES_CMDS)`

Typically:
+ A package for an application will install to `$(TARGET_DIR)` only
+ A package for a shared library will install to both `$(TARGET_DIR)` and `$(STAGING_DIR)`
+ A package for a pure header-based library (static-only) will install to `$(STAGING_DIR)` only
+ A package for a bootloader or kernel image will install to `$(BINARIES_DIR)`

## Useful Variables

In an action block:
+ `$(D)` is the source directory of the package
+ `$(MAKE)` to call make or `$(MAKE1)` to disable parallel build
+ `$(TARGET_CONFIGURE_OPTS)` and `$(HOST_CONFIGURE_OPTS)` to pass `CC`, `LD`, `CFLAGS`, etc
