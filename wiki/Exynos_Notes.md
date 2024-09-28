# Notes for AOSP on Samsung Exynos


 - IMS wont work on **SAMSUNG** devices. SLSI IMS exists but is not compatible with Samsung modem firmware.
 - Exynos devices do not use `BOARD_KERNEL_CMDLINE`. Instead, use `CONFIG_CMDLINE` and `CONFIG_CMDLINE_EXTEND=y` the kernel defconfig.
 - Bootloader errors are logged to `/proc/last_kmsg`
 - On most devices, you can use GCAM, with a few tweaks. HDR+ and Night Sight will probably not work. Use [Samsung Camera Experiments](https://github.com/TBM13/Samsung-Camera-Experiments/issues). BSG MGC is the most stable and works on most devices. AGC and SGC are finnicky. For example, BSG works fine after patching camera libs(and disabling HDR+). AGC 9.4 doesnt seem to disable HDR+? Same with SGC

## Credits: 
 - [KrutosX](https://github.com/KrutosVIP)
 - KI_4274

 _Note: I am fairly new to exynos, M34 being the first device I own. What I have put here is information collected (and probably misunderstood) by me, talking to other exynos devs._
