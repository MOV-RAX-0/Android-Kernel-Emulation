**Mission Objective:**
This section walks you through building the Android 11 common kernel (version 5.15.178) for arm64, suitable for QEMU emulation and security research.

✅ **Step 1: Confirm your environment**

Make sure you're in the `~/android-kernel-lab/common` directory:

```
cd ~/android-kernel-lab/common
```

You should already have:

* Google's prebuilt toolchain set up

* Kernel source downloaded and unpacked

* Ubuntu dependencies installed (see 00_prerequisites.md)

✅ **Step 2: Clean any previous builds**

This ensures a clean slate:

```
make mrproper
```

💡 `make clean` removes most generated files.
`make mrproper` removes everything make clean does as well as config files and editor backups. Use when resetting or switching configs.

✅ **Step 3: Configure the Kernel**

Use Google’s recommended defconfig:

```
ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- make gki_defconfig
```

This sets a known baseline configuration for a generic Android 11 kernel.

_Optional:_ Customize your build with menuconfig (e.g., to disable unused drivers):

```
ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- make menuconfig
```

✅ **Step 4: Modify KBUILD_CFLAGS to prevent build failures**

In some cases, strict warnings cause the kernel to fail compiling.
Update Makefile in the top-level kernel directory:

Find this line:

```
KBUILD_CFLAGS := -Wall -Wundef -Werror=strict-prototypes ...
```

Modify it to include:

```
KBUILD_CFLAGS += -Wno-error=unused-result -Wno-error=address -Wno-error=array-compare -Wno-error=unused-variable -Wno-error=maybe-uninitialized
```

These suppress specific warnings that cause build halts but are harmless in your QEMU emulation context.

✅ **Step 5: Build the Kernel Image**

Run the build:

```
ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- make -j$(nproc) Image
```

🛠️ If you encounter additional build errors, refer to the troubleshooting notes in `Z_Troubleshooting.md.`

✅ **Step 6: Confirm build output**

You should see:

```
ls arch/arm64/boot/Image
```

This file is your bootable kernel image for QEMU.

✅ **Step 7: (Optional) Build with debugging symbols**

If you want to inspect kernel internals with GDB:

```
ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- make -j$(nproc) Image V=1 CONFIG_DEBUG_INFO=y
```

🧠 This produces a debug-info rich kernel image suitable for step-through analysis.

✅ **Checkpoint: You now have a working kernel image.**

Next step: build a minimal initramfs so the kernel can boot.

➡️ Continue to `04_initramfs.md`

