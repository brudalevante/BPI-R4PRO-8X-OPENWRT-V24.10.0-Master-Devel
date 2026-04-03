
# BPI-R4PRO-8X-OPENWRT-V24.10.0-Master-Devel,  compile it on Ubuntu 20.04 x64 host.

![OpenWrt logo](include/logo.png)

OpenWrt Project is a Linux operating system targeting embedded devices. Instead
of trying to create a single, static firmware, OpenWrt provides a fully
writable filesystem with package management. This frees you from the
application selection and configuration provided by the vendor and allows you
to customize the device through the use of packages to suit any application.
For developers, OpenWrt is the framework to build an application without having
to build a complete firmware around it; for users this means the ability for
full customization, to use the device in ways never envisioned.

Sunshine!

## Download

Built firmware images are available for many architectures and come with a
package selection to be used as WiFi home router. To quickly find a factory
image usable to migrate from a vendor stock firmware to OpenWrt, try the
*Firmware Selector*.

* [OpenWrt Firmware Selector](https://firmware-selector.openwrt.org/)

If your device is supported, please follow the **Info** link to see install
instructions or consult the support resources listed below.

## 

An advanced user may require additional or specific package. (Toolchain, SDK, ...) For everything else than simple firmware download, try the wiki download page:

* [OpenWrt Wiki Download](https://openwrt.org/downloads)

## Development

To build your own firmware you need a GNU/Linux, BSD or macOS system (case
sensitive filesystem required). Cygwin is unsupported because of the lack of a
case sensitive file system.

### Requirements

You need the following tools to compile OpenWrt, the package names vary between
distributions. A complete list with distribution specific packages is found in
the [Build System Setup](https://openwrt.org/docs/guide-developer/build-system/install-buildsystem)
documentation.

```
binutils bzip2 diff find flex gawk gcc-6+ getopt grep install libc-dev libz-dev
make4.1+ perl python3.7+ rsync subversion unzip which
```

### Quickstart

1. Run `./scripts/feeds update -a` to obtain all the latest package definitions
   defined in feeds.conf / feeds.conf.default

2. Run `./scripts/feeds install -a` to install symlinks for all obtained
   packages into package/feeds/

3. Run `make menuconfig` to select your preferred configuration for the
   toolchain, target system & firmware packages.

4. Run `make` to build your firmware. This will download all sources, build the
   cross-compile toolchain and then cross-compile the GNU/Linux kernel & all chosen
   applications for your target system.

### Related Repositories

The main repository uses multiple sub-repositories to manage packages of
different categories. All packages are installed via the OpenWrt package
manager called `opkg`. If you're looking to develop the web interface or port
packages to OpenWrt, please find the fitting repository below.

* [LuCI Web Interface](https://github.com/openwrt/luci): Modern and modular
  interface to control the device via a web browser.

* [OpenWrt Packages](https://github.com/openwrt/packages): Community repository
  of ported packages.

* [OpenWrt Routing](https://github.com/openwrt/routing): Packages specifically
  focused on (mesh) routing.

* [OpenWrt Video](https://github.com/openwrt/video): Packages specifically
  focused on display servers and clients (Xorg and Wayland).

## Support Information

For a list of supported devices see the [OpenWrt Hardware Database](https://openwrt.org/supported_devices)

### Documentation

* [Quick Start Guide](https://openwrt.org/docs/guide-quick-start/start)
* [User Guide](https://openwrt.org/docs/guide-user/start)
* [Developer Documentation](https://openwrt.org/docs/guide-developer/start)
* [Technical Reference](https://openwrt.org/docs/techref/start)

### Support Community

* [Forum](https://forum.openwrt.org): For usage, projects, discussions and hardware advise.
* [Support Chat](https://webchat.oftc.net/#openwrt): Channel `#openwrt` on **oftc.net**.

### Developer Community

* [Bug Reports](https://bugs.openwrt.org): Report bugs in OpenWrt
* [Dev Mailing List](https://lists.openwrt.org/mailman/listinfo/openwrt-devel): Send patches
* [Dev Chat](https://webchat.oftc.net/#openwrt-devel): Channel `#openwrt-devel` on **oftc.net**.

## Linux Firmware Tarball (build dependency)

The build requires `linux-firmware-20241110.tar.xz` (≈405 MB) in the `dl/` cache.
The Makefile (`package/firmware/linux-firmware/Makefile`) is configured to download it
automatically from this repository's GitHub Release **first**, falling back to the
upstream kernel mirror if the release asset is not reachable.

### How to publish the tarball to GitHub Releases (maintainer steps)

Run these commands **once** from a machine where you already have the file and the
[GitHub CLI](https://cli.github.com) (`gh`) installed and authenticated:

```sh
# From the root of the cloned repository
gh release create firmware-20241110 \
  /path/to/linux-firmware-20241110.tar.xz \
  --repo brudalevante/BPI-R4PRO-8X-OPENWRT-V24.10.0-Master-Devel \
  --title "linux-firmware 20241110" \
  --notes "linux-firmware-20241110.tar.xz build dependency (SHA-256: 32e6d3eb5c7fcb69fe5d58976c6deafa0d6552719c6e74835064aff049d25bd7)"
```

Expected result:
- Tag: `firmware-20241110`
- Asset file name: `linux-firmware-20241110.tar.xz`
- SHA-256: `32e6d3eb5c7fcb69fe5d58976c6deafa0d6552719c6e74835064aff049d25bd7`

Once the release asset exists, any fresh clone can build without manual intervention:

```sh
make package/firmware/linux-firmware/download V=s
```

> **Do NOT** commit `dl/linux-firmware-*.tar.xz` or any other tarball directly
> into git. The `dl/` directory is listed in `.gitignore` and should stay there.

## License

OpenWrt is licensed under GPL-2.0
