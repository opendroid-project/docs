# Debugging Android

## Kernel logs

### While booted into Android
1) Run `dmesg` as root. 

### Failed boot
1) Reboot into recovery and check for `/proc/last_kmsg` or `/sys/fs/pstore/*`

## Userspace

### Stuck at boot animation (getting regular logs)
1) (Recovery) Mount System and delete file `/system/phh/secure`. Reboot.
2) Connect phone to PC.
3) Run 

    ```adb logcat -d all > logs.txt```

    while on bootsplash

### Override ADB authentication to capture logs during boot
1) Reboot into recovery.
2) Mount `system` in recovery.
3) Connect to PC.
4) Run:

    ```adb shell mount -o rw,remount /system_root```

    ```adb pull /system_root/system/build.prop```

5) Use a text editor to change the following props in build.prop 

    ```ro.adb.secure=0```
    
    ```ro.debuggable=1```
    
    ```ro.secure=0```
    
    ```persist.service.adb.enable=1```
    
    ```persist.service.debuggable=1```
    
    ```persist.sys.usb.config=adb```

    or if `GNU sed` is available on your system:

    ```
    sed -i 's/^ro\.adb\.secure=.*/ro.adb.secure=0/' build.prop
    sed -i 's/^ro\.debuggable=.*/ro.debuggable=1/' build.prop
    sed -i 's/^ro\.secure=.*/ro.secure=0/' build.prop
    sed -i 's/^persist\.service\.adb\.enable=.*/persist.service.adb.enable=1/' build.prop
    sed -i 's/^persist\.service\.debuggable=.*/persist.service.debuggable=1/' build.prop
    sed -i 's/^persist\.sys\.usb\.config=.*/persist.sys.usb.config=adb/' build.prop
    ```

6) Now copy the modified `build.prop` to /system by running:
    
    ```adb push build.prop /system_root/system/```

### Pull `/data/boot_lc_main.txt` 
1) Reboot into TWRP.
2) `adb pull /data/boot_lc_main.txt` (if it exists).

### ADB not authorized
1) Reboot into recovery.
2) Connect to PC.
3) Run:

    *Linux/Unix:*
    
    ```adb push ~/.android/adbkey.pub /data/misc/adb/adb_keys```

    *Windows:*
    
    ```adb push C:\Users\%PutHereYourUsername%\.android\adbkey.pub /data/misc/adb/adb_keys```

4) Now ADB should be allowed.

## Early-boot (bootloop at splash/OEM logo)

#### Checking pstore and last_kmsg
1) Reboot to TWRP.
2) Check

    ```/sys/fs/pstore/*```

    ```/proc/last_kmsg```

3) Copy logs to PC (Ex: `adb pull /proc/last_kmsg lastkmsg.txt`)

    **Note**: This only works if the kernel is built with 
    ```
    CONFIG_PSTORE=y
    CONFIG_PSTORE_CONSOLE=y
    CONFIG_PSTORE_PMSG=y
    CONFIG_PSTORE_RAM=y
    CONFIG_PSTORE_LAST_KMSG=y
    ```

### Using FBCon

FBCon enables kernel output to the display. Use this as a second to the last resort if the above mentioned methods don't work.

1) Build kernel with config options:
    ```
    CONFIG_VT=y
    CONFIG_FB_SIMPLE=y
    CONFIG_FRAMEBUFFER_CONSOLE=y
    CONDIG_DRM_FBDEV_EMULATION=y
    ```
2) Modify kernel cmdline to include

    ```
    androidboot.console=tty0 console=tty0
    ```

3) Reboot into fastboot or recovery.

4) Grab another phone, make some contraption to keep it pointed at screen.

5) Adjust exposure and focus to make the text visible.

6) Run
    ```
    adb reboot
    ```
    OR
    ```
    fastboot reboot
    ```
    depending on which mode you are in.

7) Pray that the recording is clear enough. If not, try again.

8) Import it into pc, flip it (if needed), start playing it slowed down and zoomed in.

## Using UART / JTAG (or better known as Serial)

This is the **ONLY WAY** to grab logs if all of the aforementioned methods do not work.

This assumes your kernel has UART logging enabled, and your DTS has it enabled.

### Disclaimer
I (the author), or OpenDroid are not responsible for any damage done to your board, whether it be a bad soldering job, scratched pcb, or fried board.

### Prerequisites
• A gadget that does UART/TTL (for most devices)

• A multimeter

• Probes (or anything that lets you reach the UART points on the board)

• Oscilloscope (if your board has undocumented serial)


1) Find your device's points

If your device has its points documented (usually somewhere in the [postmarketOS wiki](https://wiki.postmarketos.org)), then you're in luck.

If your device doesn't have the points, you can hook an oscilloscope to any points of interest and hope that your kernel has UART output enabled by default.

2) Hooking up your hardware

Attach your probes (or solder a wire to said points, ***NOT RECOMMENDED!!***)

Before plugging in your serial gadget, make sure you're using the right voltage ***AS THIS CAN FRY THE BOARD!***

3) Grabbing the logs

Fire up your serial reading program. The baud rate can be either found by cmdline passed by abl/preloader (or in defconfig.) If you can't find the baud rate then `115200` may be the default.

Now, power up your device. You may see early logs from abl/lk.

If all goes well, you should be able to find the root cause of your hang. If not, you can refer to the [postmarketOS wiki](https://wiki.postmarketos.org/wiki/Serial_debugging) as serial debugging is well documented there.
