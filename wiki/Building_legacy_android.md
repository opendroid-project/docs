For basic building instructions refer to [Building 101](/wiki/Building_101)
and incorporate the below patches after syncing or use lin-* forks.

Old Android versions can be pretty difficult to build nowadays and thus this guide was created so that you can do just that.

Assuming you set up your build enviromnet already you still have to install legacy java and python for Android 8 and below. Newer versions ship the jdk inside the source.

The [Lineage wiki](https://wiki.lineageos.org/devices/flounder/build/#java) has a fairly comprehensive guide on that for ubuntu and fedora still ships jdk8 in their repos.
Note that you might need to use [update-alternatives](https://www.baeldung.com/linux/update-alternatives-command) to set the proper symlinks and use the old java/javac version.
In addition to that you need to allow Tls V1/1.1 by removing TLSv1 and TLSv1.1 from jdk.tls.disabledAlgorithms within /etc/java-8-openjdk/security/java.security on debian based distros.

```
sudo ln -sf /usr/share/crypto-policies/LEGACY/java.txt /etc/crypto-policies/back-ends/java.config;
```
works on fedora/-based distros and does the same thing

You should verify that
```
java --version
```
and
```
javac --version
```
outputs something like openjdk/javac 1.8 for Android 7 and 8


Next is python. You need python 2 to be set as default for Android 9 and below and python 3 for Android 10 and up.
You may need to manually install the python2 package and symlink it to python using [update-alternatives](https://www.baeldung.com/linux/update-alternatives-command) just like java.
Verify the version once again using
```
python --version
```
The output should be python 2.7.xx

Now run
```
export LC_ALL=C
```
and continue the build as usual

After verifying it boots you can implement the security backports from the lineage os gerrit.
For android 7 you can repopick the topics [n-asb-2021-10](https://review.lineageos.org/q/topic:%22n-asb-2021-09%22) up to [n-asb-2024-08](https://review.lineageos.org/q/topic:%22n-asb-2024-08%22) and [tzdb_N](https://review.lineageos.org/q/topic:%22tzdb_N%22) as of writing this guide.
[lin14-mGoms](https://github.com/lin14-mGoms) is a notable fork that provides substratum, microg support and all asb backports. My personal fork [LineageOS-Revived](https://github.com/LineageOS-Revived/android/tree/cm-14.1) is mostly based on that rom.
