**Mission Objective:**

Configure the Android kernel source for cross-compilation, debugging, and memory allocator instrumentation.

🧰 **Set Architecture and Toolchain**

In your kernel source directory (e.g., ~/android-kernel-lab/common), run:

```
export ARCH=arm64
export CROSS_COMPILE=aarch64-linux-gnu-
```

You’ll need these for every build unless scripted.

🧬 **Generate a Default Config**

Start with a known baseline:

```
make defconfig
```

This creates a .config file in the root of your kernel source.

🔧 **Enable Debugging and Hardening Options**

Now launch the configuration menu:

```
make menuconfig
```

Inside the menu:

| Menu | Path |	Setting	Notes |
| --- | --- | --- |
| Kernel hacking → |	Compile the kernel with debug info	| Enables -g and line symbols |
| General setup →	| Configure standard kernel features |	Enable SLAB allocator if not set |
| General setup →	| Initramfs source file	| Leave blank unless embedding rootfs |
| Security options →	| Enable security hardening features |	E.g. KASLR, page table isolation |
| Kernel hacking → |	Magic SysRq key	| Useful for QEMU debugging |

🔒 **Optional: Disable KASLR for GDB Use**

Disable Kernel Address Space Layout Randomization for easier symbol mapping:

```
make menuconfig
```

→ `Processor type and features` → `Randomize the address of the kernel image` → `N (disable)`

You can also append `nokaslr` IN THE qemu LAUNCH COMMAND at boot time instead.

🧰 **Save and Back Up**

Once configured:

```
cp .config ../config-android11-lab
```

You’re now ready to build your kernel!
