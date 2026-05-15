# BPI-R4 PRO 8X – OpenWrt v24.10.0 (Master/Devel) fork

This repository is my OpenWrt build tree and patchset for the **Banana Pi BPI-R4 PRO 8X**.

- **Official Banana Pi forum thread (all BPI-R4 PRO 8X images, changelog, screenshots):**  
  https://forum.banana-pi.org/t/bpi-r4-pro-openwrt-v24-10-0-master-devel-source-code-on-github/26175/5  
  Images are published **by date** (the **date** in the forum post is the download link).
- Legacy / long-term notes (EN/ES): see [`LEGACY.md`](LEGACY.md)

## Author / nicknames

- Banana Pi forum nickname: **Xiaomi_ax3600**
- OpenWrt forum nickname: **bruda**  
  (On OpenWrt forums I only publish/compile images for **Banana Pi BPI-R4 4GB RAM** and **8GB RAM** in this thread):  
  https://forum.openwrt.org/t/banana-bpi-r4-all-related-to-mtk-sdk/221080/1016

![OpenWrt logo](include/logo.png)

---

## What this is / What this is not

### What this is
- A working OpenWrt build tree used to produce my **BPI-R4 PRO 8X** images
- A fork with device-specific changes for this hardware
- A place where I keep my patches, integration changes, and board-specific build adjustments
- A tree intended for the original **BE14000** card setup

### What this is not
- The official upstream OpenWrt repository
- A generic OpenWrt tree for every device
- A fork adapted for the MT7927 card variant

---

## Downloads (firmware images)

All my ready-to-flash images for **BPI-R4 PRO 8X** are published in the **official Banana Pi forum thread** linked above.

They are posted **by date**, and the **date itself** is the download link.

If you are looking for official upstream OpenWrt firmware for other devices, use:

- https://firmware-selector.openwrt.org/

---

## Build environment

This fork is typically built on **Ubuntu 20.04 x86_64** with a **case-sensitive filesystem**.

### Requirements

Package names may vary depending on the Linux distribution. See the official OpenWrt documentation:

- https://openwrt.org/docs/guide-developer/build-system/install-buildsystem

Typical package list:

```text
binutils bzip2 diff find flex gawk gcc-6+ getopt grep install libc-dev libz-dev
make4.1+ perl python3.7+ rsync subversion unzip which
```

---

## Quickstart

1. Clone the repository:

   ```sh
   git clone https://github.com/brudalevante/BPI-R4PRO-8X-OPENWRT-V24.10.0-Master-Devel.git
   cd BPI-R4PRO-8X-OPENWRT-V24.10.0-Master-Devel
   ```

2. Update feeds:

   ```sh
   ./scripts/feeds update -a
   ```

3. Install feeds:

   ```sh
   ./scripts/feeds install -a
   ```

4. Configure:

   ```sh
   make menuconfig
   ```

5. Pre-download sources:

   ```sh
   make download -j1 V=s
   ```

6. Build:

   ```sh
   make -j"$(nproc)" V=s
   ```

---

## Optional: faster staged compile with log output

If you want to pre-build key parts using all CPU cores and keep a full log for troubleshooting, you can use:

```sh
make -j"$(nproc)" {toolchain,target,package/firmware/linux-firmware}/compile V=s 2>&1 | tee compile.log
```

If a build error occurs, you can quickly locate the failing stage with:

```sh
grep "failed to build" compile.log
```

This step is optional, but it can speed up preparation and make troubleshooting easier.

---

## Required linux-firmware archive

This build requires the following file:

```text
linux-firmware-20241110.tar.xz
```

Because of its large size (around **405 MB**), it is provided through this repository's **GitHub Releases** as a release asset instead of being committed directly into the repository.

### Required action

Download the release asset:

```text
linux-firmware-20241110.tar.xz
```

from this repository's **Releases** section and place it manually in:

```text
dl/
```

### Important

Do **not** use GitHub's automatically generated:

- **Source code (.zip)**
- **Source code (.tar.gz)**

downloads, because they are **not** the required firmware archive.

A clean build may **not download this file automatically** in this setup, so it must be added manually to `dl/` before compiling.

---

## Required Airoha firmware files

This tree includes the required Airoha firmware files for this build:

```text
EthMD32.dm.bin
EthMD32.DSP.bin
```

---

## Feeds

This repository uses the normal feed workflow for the original **BE14000** card setup:

```sh
./scripts/feeds update -a
./scripts/feeds install -a
```


---

## Board-specific changes included in this fork

This repository contains board-specific modifications for this hardware variant, including changes in areas such as:

- `target/linux/mediatek/files-6.6/arch/arm64/boot/dts/mediatek/`
- `target/linux/mediatek/image/filogic.mk`
- board integration changes for the original BPI-R4 PRO 8X setup

These changes are part of the fork and are required for this specific build target.

---

## Upstream OpenWrt documentation

- Supported devices database: https://openwrt.org/supported_devices
- Quick Start: https://openwrt.org/docs/guide-quick-start/start
- User Guide: https://openwrt.org/docs/guide-user/start
- Developer docs: https://openwrt.org/docs/guide-developer/start

---

## License

OpenWrt is licensed under **GPL-2.0**.
