# GKI LXC

Enable LXC support for gki kernel, everything comes from [Common-Android-Kernel-Tree](https://github.com/lateautumn233/Common-Android-Kernel-Tree), Based on [android12-5.10](https://source.android.com/docs/core/architecture/kernel/gki-android12-5_10-release-builds)

# Build

Build reference [KernelSU](https://kernelsu.org/guide/how-to-build.html)
```bash
# Sync the kernel source code
mkdir android-kernel; cd android-kernel
repo init --depth 1 -u https://android.googlesource.com/kernel/manifest -b [BRANCH]
repo sync
```

Clone this repo
```bash
git clone https://github.com/TapetalArray/gki-lxc
```

Apply patches and configuration files
```bash
cp ./gki-lxc/gki_defconfig ./android-kernel/common/arch/arm64/configs/gki_defconfig
cd android-kernel/common
git apply ../../gki-lxc/*.patch
```

Build
```bash
LTO=thin BUILD_CONFIG=common/build.config.gki.aarch64 build/build.sh
```

Create img file
raw compression format
Download boot from [this](https://source.android.com/docs/core/architecture/kernel/gki-android12-5_10-release-builds)
```bash
./tools/mkbootimg/unpack_bootimg.py --boot_img ../boot-5.10-xxxx.img
./tools/mkbootimg/mkbootimg.py --header_version 4 --kernel out/android12-5.10/dist/Image --ramdisk out/ramdisk --os_version [OS_VERSION] --os_patch_level [OS_PATCH_LEVEL] -o ../android12-5.10.xxx_[OS_PATCH_LEVEL]-boot.img
```

# Credits

[lateautumn233](https://github.com/lateautumn233)
