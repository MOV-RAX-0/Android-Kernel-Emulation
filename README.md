# Android-Kernel-Emulation
Android kernel emulation using QEMU for fun and education. This is a lab-focused initiative dedicated to Android kernel emulation, debugging, and vulnerability exploration.  This repository documents a reproducible, end-to-end process for emulating Android kernels using QEMU, instrumenting them with GDB, and experimenting with SLAB allocator behavior for educational purposes.

# Purpose
* Showcase hands-on kernel security research
* Build repeatable labs for Android kernel emulation and fuzzing
* Understand `kmem`, SLAB behavior, freelist reuse, and kernel hardening
* Contribute meaningful documentation to the security community

# Learning Goals
* Compile Android kernels with debug symbols
* Run them in QEMU with custom initramfs
* Trace memory allocation via `kmalloc`, `kmem_cache_*`, etc.
* Use `/proc/slabinfo`, GDB, and symbol maps to interpret behavior
* Study kernel memory bugs (e.g. SLAB UAF)

# License
This project is licensed under the terms of the MIT license. **It is provided for educational and research purposes only.** Do not use it in violation of laws, terms of service, or ethical guidelines.

# Credits
Created by MOV_RAX_0
If this project helped you, consider giving it a ⭐️ or submitting a PR to expand the documentation!
🫡 _Ad Astra, Simul_
