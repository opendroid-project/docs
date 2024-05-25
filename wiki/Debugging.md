## HOW TO TAKE LOGS

### Stuck on boot animation 
(or need regular logs)
- in twrp mount System and delete file /system/phh/secure and then reboot
- while you see bootanimation connect phone to PC
- adb logcat -d all > logs.txt

### Stuck on bootanimation/splash 
(or need regular logs but ADB is dead)
- reboot into TWRP
- `adb pull /data/boot_lc_main.txt` (if it exist)

### ADB not allowed
(device unauthorized)
- reboot to twrp
- connect phone to the PC
- run command from

*nix: 
```adb push ~/.android/adbkey.pub /data/misc/adb/adb_keys```

windows: 
```adb push C:\Users\%PutHereYourUsername%\.android\adbkey.pub /data/misc/adb/adb_keys```

- now you can take logcat logs

### Stuck/bootloop at splash
- reboot to twrp right after stuck-splash or bootloop
- try to find logs here:

`/sys/fs/pstore/console-ramoops`

`/proc/last_kmsg`

- copy logs to PC
(for sample, with command "adb pull /proc/last_kmsg lastkmsg.txt")

Source: Mystic GSI notes