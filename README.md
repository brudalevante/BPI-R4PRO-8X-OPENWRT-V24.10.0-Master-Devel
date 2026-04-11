# BPI-R4 PRO 8X – OpenWrt v24.10.0 (Master/Devel) fork

This repository is my OpenWrt build tree and patchset for the **Banana Pi BPI-R4 PRO 8X**.

- Official forum thread (downloads/changelog/screenshots):  
  https://forum.banana-pi.org/t/bpi-r4-pro-openwrt-v24-10-0-master-devel-source-code-on-github/26175/5
- Legacy / long-term notes (EN/ES): see [`LEGACY.md`](LEGACY.md)

![OpenWrt logo](include/logo.png)

---

## What this is / What this is not

**What this is**
- A working OpenWrt build tree used to produce my BPI-R4 PRO 8X images.
- A place where I keep device-specific patches (SFP/PON, switch/Wi-Fi integration, etc.).

**What this is not**
- Upstream OpenWrt official repository.
- A “general purpose” OpenWrt support repo for all devices.

---

## Downloads (firmware images)

I publish ready-to-flash images **by date** in the official Banana Pi forum thread above.  
(The **date** in the post is the download link.)

If you are looking for official upstream OpenWrt images for other devices, use:
- https://firmware-selector.openwrt.org/

---

## Build environment

This fork is typically built on **Ubuntu 20.04 x86_64** (case-sensitive filesystem required).

### Requirements

Package names vary by distro; see OpenWrt documentation:
- https://openwrt.org/docs/guide-developer/build-system/install-buildsystem

Typical tool list:
```
binutils bzip2 diff find flex gawk gcc-6+ getopt grep install libc-dev libz-dev
make4.1+ perl python3.7+ rsync subversion unzip which
```

### Quickstart

1. Update feeds:
   ```sh
   ./scripts/feeds update -a
   ```
2. Install feeds:
   ```sh
   ./scripts/feeds install -a
   ```
3. Configure:
   ```sh
   make menuconfig
   ```
4. Build:
   ```sh
   make -j"$(nproc)"
   ```

---

## Linux Firmware Tarball (build dependency)

The build requires `linux-firmware-20241110.tar.xz` (≈405 MB) in the `dl/` cache.

The Makefile (`package/firmware/linux-firmware/Makefile`) is configured to download it
automatically from this repository's GitHub Release **first**, falling back to the
upstream kernel mirror if the release asset is not reachable.

### How to publish the tarball to GitHub Releases (maintainer steps)

Run these commands **once** from a machine where you already have the file and the
GitHub CLI (`gh`) installed and authenticated:

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

> Do **NOT** commit `dl/linux-firmware-*.tar.xz` (or any other tarball) into git.  
> The `dl/` directory is in `.gitignore` and should stay that way.

---

## Support / Documentation (upstream)

- Supported devices database: https://openwrt.org/supported_devices
- Quick Start: https://openwrt.org/docs/guide-quick-start/start
- User Guide: https://openwrt.org/docs/guide-user/start
- Developer docs: https://openwrt.org/docs/guide-developer/start

---

## License

OpenWrt is licensed under GPL-2.0
