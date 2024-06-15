For basic building instructions refer to [Building 101](/wiki/Building_101)
and incorporate the below patches after syncing or use LineageOS-UL.

The following article pertains to the optimization of legacy devices,
specifically those operating on versions equal to or below 3.18.

Navigating the intricacies of legacy chipsets, especially with the
introduction of obstacles such as BPF requirements by Google, can be a
daunting task. However, to facilitate the booting process for A12 and
beyond, a comprehensive list of essential patches has been compiled.

Graciously, these patches have been submitted to the LineageOS Gerrit
for Android 12

- [twelve-ultralegacy-devices](https://review.lineageos.org/q/topic:%22twelve-ultralegacy-devices%22)
- external/perfetto 287706
- [twelve-restore-camera-hal1](https://review.lineageos.org/q/topic:%22twelve-restore-camera-hal1%22)
- [twelve-camera-extension](https://review.lineageos.org/q/topic:%22twelve-camera-extension%22)
- [twelve-legacy-camera](https://review.lineageos.org/q/topic:%22twelve-legacy-camera%22)
- [twelve-qcom-cam](https://review.lineageos.org/q/topic:%22twelve-qcom-cam%22)
- [twelve-qcom-legacy-sepolicy](https://review.lineageos.org/q/topic:%22twelve-qcom-legacy-sepolicy%22)
- system/sepolicy 324005, 324006
- hardware/lineage/interfaces 325387

Since not all roms track lineageos gerrit, I made a general script
to adapt most roms for legacy devices:
<https://github.com/Meghthedev/patchscript/blob/main/patchanysource.sh>


The preferred way nowadays is using the [LineageOS-UL](https://github.com/LineageOS-UL) source. This ROM contains all the necessary
patches to make 3.4 devices boot A12 up to A14. It has been successfully tested on a device powered by the msm8916 and msm8974 chipset.

Commits to boot a13/a14 on msm8916/3.10 are here:
<https://github.com/acroreiser/android_device_lenovo_a6010>
<https://gitlab.com/TipzTeam/android-trees/android_device_cyanogen_msm8916-common>

msm8974/3.4 reference to make a14 work can be found here
<https://github.com/z3DD3r/android_device_lge_hammerhead>
