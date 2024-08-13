# Bring-up a device from scratch

#### Sad backstory

---

***So, your device has no ROMs and no developer out there is willing to work on it because it's shitty with a shitty SoC from the shitty xxx company, sad.*****

> Shit, I'm the same as you but the difference between us is that I managed to gather my courage and actually make this myself. So gather your courage as well and prove that you're useful at least once.

## Okay, let's do this 🔥 🔥 wait... how do I even start?

> Damn... you got me there, joking. Let's actually start now...

**Well, let's break this down to a few simple steps:** *ugh, they always say that before starting to yap in nerd language.*

> ~~Actually 🤓 I'm not a nerd~~ ah shit... anyways:

1. Get info about your device.
2. Get a direct download link to your device's firmware* and throw it my dumper bot's way on [Telegram]https://t.me/OkBuddyGSI).
3. Wait for the dump* to finish, as it's dumping you might wanna go touch some grass (you need it man, I hear your body screaming from here).
4. Get the device tree* generated automatically by our dumper service.
5. Push it to GitHub*. *what? is this like that yellow...*
   > NO NO, NOT THAT! I'll explain that in detail, later.
6. Welcome to hell! *What kind of guide is this? I'm outta here*
   > No, wait. I'll get serious I PROMISE! if you leave they will ... me

#### Detailed steps:

---

1. ##### ***Device info.***

* * Google your device's name [example](assets/20240813_164030_image.png)
* GSMArena will provide you with most of the info you need but a more robust way is to use this [app](https://play.google.com/store/apps/details?id=ru.andr7e.deviceinfohw&hl=en)

2. ##### ***Device firmware.***

* Search google for that too. [example2](assets/20240813_165746_image.png)
* This website works well for me [example3](assets/20240813_165929_image.png) It provides MediaFire links which work great with our dumping service.

3. ##### ***Touching grass.***

* Go outside, find this green thing and *touch* it [Couple touching grass (they have a life 0:)](assets/has-a-life.jpg)
  > Note: if you're an arch user then you probably thought I'm referring to the *touch* linux command.. NO, what I meant is, go outside and make a physical contact between your soft femboy hand and the green material in the picture.

4. ##### ***Getting the device tree generated automatically.***

* You will find a post like this with your device's codename* if the dump was successful. [example4](assets/20240813_173412_image.png)
* Enter this directory. [example5](assets/20240813_174645_image.png)
* Download this tree. [example6](assets/20240813_174808_image.png)

5. ##### ***Pushing to GitHub.***

* Make a GitHub account if you don't have one already. *how do I make one?*
  
  > Pff, I won't explain to you how to make a GitHub account!
* Make a new code repositroy* (repo for short). You can do so by going to the "Repositories" tab and clicking on this green button. [example7](assets/20240813_180248_image.png)
  
  > speaking of green, did you touch grass earlier?
* In the next page, you can choose any name but I recommend this naming scheme: `android_device_manufacturer_codename` leave everything else as default.
* Now, let's install *git*, shall we?
  
  1. **Go to *git*'s official [website](https://git-scm.com/)**
  2. **Download the installer like this. [example8](assets/20240813_181752_image.png)**
  3. **Run the downloaded executable.**
  4. **Spam the *next* button.**
  5. **Wait for the installation to complete.**
  6. **Reboot.**

> ***Note: Those steps are for installing *git* only on Winblows.***

* Open CMD in the directory where you extracted the device tree source zip, you can do so by typing `cmd` on the path section in file explorer. [example9](assets/20240813_183042_image.png)
* Insert and execute the following commands one by one.
  1. `git init`
  2. `git add .`
  3. `git commit -m "blah blah"` put whatever you want in ***blah blah*** place
  4. `git branch -M main`
  5. `git remote add origin link` where ***link*** is the link to the repo you created earlier

6. ##### ***Welcome to the Android ROM development community.***

* You will probably curse me a year from now cuz I probably introduced you the top of the *ice*berg of *hell*.

> **NOTE**: ***EVERY WORD THAT HAS * AFTER IT IS AN EXPRESSION WE USE THAT I WILL EXPLAIN IN A LATER GUIDE.***

