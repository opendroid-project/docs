# Building 101

This guide aims to teach users about the basics of android building.
If you want to build A12+ for devices not using linux 4.4 and up please refer to [Building android for legacy devices](/wiki/Building_android_for_legacy_devices)
For versions unsupported by AOSP refer to [Building Legacy Android](/wiki/Building_legacy_android)

Do not follow this guide inside Crave Devspace CLI, follow [this](/wiki/Crave_Devspace) instead because this guide shows things that are against [Devspace Rules](/wiki/Crave_Rules)

## Syncing AOSP Source code

### Setup: Installing `repo` and other dependencies

Repo is a tool developed by google to help manage the thousands of git repos used in a project like AOSP.

You must first install it on your computer/server.
```
mkdir ~/bin && curl https://storage.googleapis.com/git-repo-downloads/repo > ~/bin/repo && chmod a+x ~/bin/repo
```
After that you should check if ~/bin is already in your PATH by doing
```
printenv PATH|grep ~/bin
```
if you do not get any output you have to add:
```
# set PATH so it includes user's private bin if it exists
if [ -d "$HOME/bin" ] ; then
    PATH="$HOME/bin:$PATH"
fi
```
to ~/.profile or some other script that runs on shell creation.

There are also some additional dependencies, which you need to install to build android.

Arch Linux:
```
sudo pacman -S repo base-devel git-lfs
```

Debian / Ubuntu based distros:
```
sudo apt install bc bison build-essential ccache curl flex g++-multilib gcc-multilib git git-lfs gnupg gperf imagemagick lib32ncurses5-dev lib32readline-dev lib32z1-dev liblz4-tool libncurses5 libncurses5-dev libsdl1.2-dev libssl-dev libwxgtk3.0-gtk3-dev libxml2 libxml2-utils lzop pngcrush rsync schedtool squashfs-tools xsltproc zip zlib1g-dev
```

Fedora / Fedora based distros:
```
sudo dnf install @development-tools android-tools automake bison bzip2 bzip2-libs ccache curl dpkg-dev flex gcc gcc-c++ git git-lfs glibc-devel.{x86_64,i686} gperf libstdc++.{x86_64,i686} libxcrypt-compat libxml2-devel libxslt lz4-libs lzop make maven ncurses ncurses-compat-libs ncurses-devel.{x86_64,i686} openssl openssl-devel pngcrush python python3-virtualenv python3 python3-mako python-mako python-networkx schedtool squashfs-tools syslinux-devel unzip zip zlib zlib-devel
```

You might need to remove metadata_csum_seed and orhpan_file from /etc/mke2fs.conf if you encounter error mentioning "Invalid filesystem option set" as per [Reddit](https://www.reddit.com/r/LineageOS/comments/18lej4b/if_your_build_is_failing_with_an_error_regarding/)


If you build a lot you should consider enabling ccache to save yourself some build time.
```
export USE_CCACHE=1
export CCACHE_EXEC=/usr/bin/ccache
```
You should limit its size to something reasonable with the below command
```
ccache -M 50G
```
To further increase the number of cached files at the cost of slight decompression overhead you can enable compression.
```
ccache -o compression=true
```

### Initialising and syncing source

1. Find the manifest of your preferred android fork. This guide will be using LineageOS as an example.

2. Make a directory in which you want the android source code to be cloned
    ```
    mkdir Lineage
    ```

3. Cd into the directory
    ```
    cd Lineage
    ```

4. Run
    ```
    repo init -u https://github.com/LineageOS/android.git -b lineage-21.0 --git-lfs
    ```

    Replace the url with the link to your preferred source of android. Replace the branch with the version of android you want to build. (Lineage 21 is A14, 20 is A13)

    - **Note #1**: If you get an error like `repo does not know who you are`, run 
        ```
        git config --global user.name "ProAndroidBuilder" # This can be any name, even your real name.
        ```

        ```
        git config --global user.email "android@veryrealemail.com" # This should be your email.
        ```
        Be warned, these info will be public if you start committing and pushing things.

    - **Note #2**: You can specify the `--depth=1` argument here to save storage space albeit at the loss of commit history.

5. Run
    ```
    repo sync -c --force-sync --optimized-fetch --no-tags --no-clone-bundle --prune -j$(nproc --all) --retry-fetches=25
    ```

In case you get RPC errors, try reducing the -j argument to something like four. I usually run this sync command two or three times, to clone any repos that failed to clone during the previous sync (bad internet problems).

6. Wait for the command(s) to complete.

Excellent! You have now successfully cloned android!

### Cloning device repositories.

In order to build android for a device, you must first clone a few things:

* Device tree
* Vendor tree
* Kernel tree
* Other hardware/ repos

The exact repos to clone varies from device to device. Some devices may use a common tree (which means that you have to clone 2 device and vendor trees), while others (especially samsung trees) have many hardware/ dependencies.

Sometimes, the device maintainer might have been kind enough to share a "local manifest". This basically allows you to clone the trees hassle-free with `repo`.

If you are lucky enough to have a local manifest, just drop it in to 
```
ANDROID_CLONE_DIRECTORY/.repo/local_manifests/
```
(you may have to create this directory). After this, just run repo sync once more. You can also do this step right after repo init to save yourself another sync.

---

If you arent lucky enough to have a local manifest, do not worry, you can still clone the repos manually.

Run `git clone https://url -b branch(optional) path/to/folder`. The exact repos you need to clone will often be mentioned in a lineage.dependencies, aosp.dependencies or crdroid.dependencies file within the device tree.

Vendor repos are usually not listed, but try to get them from the same source as the device trees. For official Lineage OS this TheMuppets [GitLab](https://gitlab.com/the-muppets)/[GitHub](https://github.com/TheMuppets), but you can also extract the blobs manually if you have a build with that tree flashed onto your device by running:

```
./extract-files.sh
```
or
```
./extract-files.py
```

in your device tree when connected to your device with enabled adb root. Alternatively you can also [extract them from the rom zip](https://wiki.lineageos.org/extracting_blobs_from_zips)

**Note**: Most people name their android-related repositories like `android_device_xiaomi_spes` or `device_xiaomi_spes`. You can easily find the path you need to  clone the repo to by replacing the `_` with a `/`, ignoring prefixes like android_ and proprietary_


## Building Time!

1. Run
    ```
    . build/envsetup.sh
    ```
    or
    ```
    source build/envsetup.sh
    ```

2. Run `lunch PRODUCT-RELEASE(IF A14 QPR2+)-VARIANT`. This is the device and variant you want to build for. 
(eg `lunch lineage_bacon-userdebug or lunch lineage_oxygen-ap2a-userdebug)

However, it is easier to use breakfast instead, like `breakfast PRODUCT VARIANT` 
(eg. breakfast oxygen userdebug)

    - *Product*: Many roms have their own prefixes. Lineage and most lineage-based forks use lineage_device, LMOdroid uses lmo_device and AOSP based forks use aosp_device.

    - *Release*: **Post A14 QPR2**, you must include the `release` field. On A14 QPR2, this will look something like `ap1a`. On android `main` branch, this is `trunk_staging`.

    - *Variant*: Variant is the type of build you want to build.
        - `eng` offers easy debugging, but runs slow on the device and is insecure. This should ideally only be used for tree developement or booting a new android version for the first time on a device.

        - `userdebug` is a hybrid between `eng` and `user`. This offers easier debugging than `user`, like `adb root`(if enabled through settings) and runs faster on the device. This builds slower. Most people ship custom roms as this variant.

        - `user` is the least debuggable variant. Most OEMs ship their stock rom in this variant. Here debuggability is minimal.

3. Run
    ```
    m -j$(nproc --all)
    ```

    - **Note #1**: If you get errors regarding memory, try reducing the -j argument. The j (jobs) argument is the number of threads which are used during compilation.

    - **Note #2**: If you want a flashable zip instead of raw images, run (for most roms)
    ```
    m bacon -j$(nproc --all)
    ```

Hopefully, android builds without errors. This guide does not cover fixing common errors. Once done, you should hopefully see `build completed`.

## Sharing the build

The built rom is stored in `out/target/product/YOUR DEVICE CODENAME`. 
(eg. out/target/product/bacon)

Depending on which target you chose, the files here will be different.
Simply share your *.img files or the latest .zip file in that folder. Please ensure to have your build(s) thoroughly tested before you publicly release them.

If your rom does not boot, ask other maintainers for your device if your device needs specific patches or refer to [Debugging](/wiki/Debugging) in order to grab logs.
