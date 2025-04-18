**Mission Objective:**
Retrieve a supported Android kernel source and prepare it for cross-compilation and emulation.

**Target Overview**
This guide assumes you are targeting an **ARM64 kernel** either:
* A generic Android kernel, or
* A device-specific kernel (e.g. Samsung S21, Pixel 6)

We'll start with a generic Android kernel for educational use:

  **Fetch the Kernel Source (Generic Android 11)**

```
cd ~/android-kernel-lab
git clone https://android.googlesource.com/kernel/common.git
cd common
```
List all available branches:

```
git ls-remote --heads origin | grep android
```

Checkout the branch you would like (e.g. android11-5.4-lts)

```
git checkout origin/android11-5.4-lts -b android11-5.4-lts
```

**Optional: Using a Device-Specific Kernel**

If you're targeting a specific phone, like a Pixel 6a, clone the correct kernel:

```
git clone https://android.googlesource.com/kernel/google-modules/pixel-6
```

Or from your vendor's GitHub page (e.g., Samsung, Oneplus).

**Folder Recap**
Your directory should look something like this:

```
~/android-kernel-lab/
├── common/                ← Your working kernel source
└── initramfs/             ← Root filesystem used for QEMU boot
```
