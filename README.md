# Ubuntu Touch adaptation for the Fxtec Pro1-X (QX1050)

This is based on Halium 11.0, and uses the mechanism described in [this page](https://github.com/ubports/porting-notes/wiki/GitLab-CI-builds-for-devices-based-on-halium_arm64-(Halium-9)).

This project can be built manually (see the instructions below) or you can download the ready-made artifacts from GitLab: take the [latest archive](https://gitlab.com/ubports/porting/community-ports/android11/fxtec-pro1x/fxtec-pro1x/-/jobs/artifacts/halium-11.0/download?job=devel-flashable), unpack the `artifacts.zip` file (make sure that all files are created inside a directory called `out/`, then follow the instructions in the [Install](#install) section.

## How to build
To manually build this project, follow these steps:

```bash
./build.sh -b workdir  # workdir is the name of the build directory
./build/prepare-fake-ota.sh out/device_pro1x.tar.xz ota
./build/system-image-from-ota.sh ota/ubuntu_command out
```

## Install
 1. Unlock the device bootloader following the usual unlock procedure (enable Developer mode, turn on "OEM unlocking" option, then reboot to bootloader and execute `fastboot flashing unlock`)
 2. Reboot to bootloader, format userdata (if switching from Android) and flash recovery.img from the build artifacts, then reboot to fastbootd mode:
```bash
fastboot format:ext4 userdata
fastboot --set-active=a
fastboot flash recovery out/recovery.img
fastboot reboot fastboot
```
 3. Flash system.img and boot.img, then reboot:
```bash
fastboot flash system_a out/system.img
fastboot flash boot_a out/boot.img
fastboot flash dtbo_a out/dtbo.img
fastboot reboot
```
