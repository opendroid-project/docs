The following article pertains to the optimization of legacy devices,
specifically those operating on versions equal to or below 3.18.

Navigating the intricacies of legacy chipsets, especially with the
introduction of obstacles such as BPF requirements by Google, can be a
daunting task. However, to facilitate the booting process for A12 and
beyond, a comprehensive list of essential patches has been compiled.

Graciously, these patches have been submitted to the LineageOS Gerrit
for Android 12

- art 318097
- external/perfetto 287706
- 321934
- system/bpf 320591
- system/netd 320592
- 326385
- twelve-restore-camera-hal1
- twelve-camera-extension
- 320528-320530
- hardware/interfaces 320531-320532
- twelve-legacy-camera
- max-mag-field
- 320333
- 324005-324006
- 325387

But, not all roms tracked lineageos gerrit, so i made a general script
to adapt any rom for legacy devices:
<https://github.com/Meghthedev/patchscript/blob/main/patchanysource.sh>

Although the script has not been updated for A13, a noteworthy
alternative has emerged in the form of LineageOS-UL
(https://github.com/LineageOS-UL). This ROM contains all the necessary
patches to enable the booting of 3.10 devices on A14. It has been
successfully tested on a device powered by the MSM8916 chipset.

Commits to boot a14 on msm8916 are here:
<https://github.com/acroreiser/android_device_lenovo_a6010/> you can
adapt them for your own chipset. another reference for a13 is
<https://gitlab.com/TipzTeam/android-trees/android_device_cyanogen_msm8916-common>