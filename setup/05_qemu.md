

**Mission Objective:**
Boot the Android 11 ARM64 kernel (version 5.15.178) using QEMU, with an `initramfs` and support for file transfer from the host system. This validates that the kernel was built correctly and is able to initialize a basic root filesystem.

---

## 🧭 Step 1: Launch QEMU with Initramfs (no GDB yet)

Ensure you're in the `~/android-kernel-lab/common` directory and that your kernel image and `initramfs.cpio.gz` are both present there.

```
cd ~/android-kernel-lab/common

qemu-system-aarch64 \
  -machine virt \
  -cpu cortex-a53 \
  -nographic \
  -smp 2 \
  -m 1024M \
  -kernel arch/arm64/boot/Image \
  -initrd initramfs.cpio.gz \
  -append "console=ttyAMA0 earlycon=pl011,0x09000000 rdinit=/init nokaslr"
```

If successful, you should see a BusyBox shell prompt.

---

## 📦 Step 2: Mount `proc` and explore

After logging in to the shell (you will not see a login prompt, just `~ #`), try mounting and using basic commands:

```sh
mount -t proc proc /proc
```

Try a few more:

```sh
uname -a
cat /proc/cpuinfo
```

If `ls` is not found, ensure that:
- BusyBox was built with `ls` applet enabled
- It is symlinked in your `/bin`

---

## 🔁 Step 3: Enable file transfer (host ↔ emulated kernel)

### 📁 Step 3.1 — Create a shared host directory
```bash
mkdir -p ~/android-kernel-lab/shared-dir
echo "Hello from host" > ~/android-kernel-lab/shared-dir/test.txt
```

### 🛰 Step 3.2 — Re-launch QEMU with 9p mount
```bash
qemu-system-aarch64 \
  -machine virt \
  -cpu cortex-a53 \
  -nographic \
  -smp 2 \
  -m 1024M \
  -kernel arch/arm64/boot/Image \
  -initrd initramfs.cpio.gz \
  -append "console=ttyAMA0 earlycon=pl011,0x09000000 rdinit=/init rootfstype=9p rootflags=trans=virtio" \
  -fsdev local,id=fsdev0,path=./shared-dir,security_model=none \
  -device virtio-9p-device,fsdev=fsdev0,mount_tag=hostshare
```

### 📂 Step 3.3 — Mount it inside the guest
```sh
mkdir -p /mnt/hostshare
mount -t 9p -o trans=virtio hostshare /mnt/hostshare
```

Then verify:
```sh
cat /mnt/hostshare/test.txt
```

✅ You now have bidirectional file transfer between the host and QEMU kernel environment!

---

## 🛠 Step 4: Common issues & fixes

| Symptom | Fix |
|--------|-----|
| `No working init found` or `can't execute /init` | Check if BusyBox was compiled for the correct `ARCH`, statically linked, and that `init` is executable. |
| `mount: not found` | Rebuild BusyBox with `mount` applet enabled. |
| `cd /mnt/hostshare: No such file or directory` | Ensure you created the directory before attempting to mount it. |
| `cannot initialize fsdev 'fsdev0'` | Confirm the shared directory exists on the host and path is correct. |

---

## ✅ Checkpoint Summary
- Kernel boots in QEMU with early console and BusyBox shell
- `initramfs.cpio.gz` contains a valid `/init` script and symlinked applets (e.g., `sh`, `ls`, `mount`)
- `mount -t proc proc /proc` works
- File transfer between host and guest confirmed via 9p mount

---

➡️ Next: `05_debug.md` — Boot with `-s -S` for attaching GDB and analyzing kernel internals!

