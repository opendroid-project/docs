# Building Android from start to finish

## Getting started

Earlier, Cirrus CI gave the ROM Builders community servers for OSS developement of Android builds, as these builds need a lot of resources that most people don't have lying around, like double digit RAM requirements (GB), triple digit storage requirements (GB), a capable processor, a specific Linux environment, etc. 

Cirrus CI has discontinued ROM Builders around Nov 2023. Now, [crave.io](https://crave.io) is providing Build Servers with a pretty similar Queue System. It is also a lot more flexible as it allows custom commands, has Docker image support, allows entering into build storage without starting a build, etc.

This guide attempts to help new and old users with Android development and using crave.io through Devspace CLI and alternatively, Github Actions.

Crave Resources:
- [Crave Devspaces CLI](/wiki/Crave_Devspace)
- [Crave Devspaces CLI - Rules](/wiki/Crave_Rules)
- [Crave Devspaces CLI - Additional Tips and Tricks](/wiki/Crave_Tricks)
- [Crave Devspaces CLI - Signing Builds(Advanced/WIP)](/wiki/Crave_Signing)
- [Crave AOSP Builder (Github Actions)](https://github.com/sounddrill31/crave_aosp_builder)

Information and Guides:
- [Building 101](/wiki/Building_101)
- [Glossary of key terms](/wiki/Glossary)
- [Debugging/Taking Logs](/wiki/Debugging)
- IMY's Bringup Guides
  - [Bringup](/wiki/bringup)

Useful Resources:
- [Lineage Build Guide](https://wiki.lineageos.org/devices/bacon/build)
- [Lopestom's Bringup guides](https://gist.github.com/lopestom/)
- [RealOGs Bringup Guide](https://blog.realogs.in/android-device-tree-bringup)
- [AlaskaLinuxUser Youtube](https://www.youtube.com/channel/UCnGqG_jyyXmTzdamBpKfeHA)
- [AlaskaLinuxUser Blog](https://alaskalinuxuser3.ddns.net/)
- [Awesome Android AOSP](https://github.com/Akipe/awesome-android-aosp/blob/main/readme.md)
- [LineageOS Vendor Tree Guide](https://wiki.lineageos.org/proprietary_blobs.html)

GSI and more Resources:
- [Phhusson's Treble-Experimentations](https://github.com/phhusson/treble_experimentations/wiki)

Signing Guides(Non-Crave):
- [Inline Guide for Dev-Keys by A2L5E0X1](https://gist.github.com/A2L5E0X1/54cb1b3a49030a9ebf8608b4e68073f5)
- [Inline Guide by CrDroid](https://crdroid.net/blog/2024-06-01-sign-your-crDroid-builds-and-keep-play-integrity-happy)
- [Detailed Guide by LineageOS](https://wiki.lineageos.org/signing_builds)
- [Detailed Guide by AOSP](https://source.android.com/docs/core/ota/sign_builds)

SELinux Guides:
- [AOSP General SELinux](https://source.android.com/security/selinux/customize)
- [AOSP Device SELinux](https://source.android.com/security/selinux/device-policy)
- [LineageOS SELinux Guide](https://lineageos.org/engineering/HowTo-SELinux)

Tools:
- [Azwhikaru's Action-TWRP-Builder](https://github.com/azwhikaru/Action-TWRP-Builder)
- [SebaUbuntu's Twrpdtgen](https://github.com/twrpdtgen/twrpdtgen)
- [SebaUbuntu's Aospdtgen](https://github.com/sebaubuntu-python/aospdtgen)
- [DumprX](https://github.com/DumprX/DumprX)
- [cd-Crypton's extract_proprietary_blobs](https://github.com/cd-Crypton/extract_proprietary_blobs)

Chat Groups:
- [Get Help](/wiki/Get_Help)
