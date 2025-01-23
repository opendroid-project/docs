# Getting Started

## Setting Up Crave Devspace CLI

### Reading the Rules

Please read the rules before getting started.

[Here's the link to the rules](Crave_Rules)

### Getting a [foss.crave.io](https://Foss.crave.io) account

 Please request an account [here](https://forms.gle/Jhvy9osvdmcS9B7fA) since self-signup is disabled.

Also check out our [discord](https://discord.crave.io)!

### General Workflow

To use crave, you'll likely be doing this:
- Enter Devspace CLI ([Guide](/wiki/Crave_Devspace#downloading))
- Crave Cloning a new project and entering that folder([Guide](/wiki/Crave_Devspace#setting-up-the-project))
- Set up workspace Persistance ([Guide](/wiki/Crave_Devspace#workspace-persistence))
- Using Crave Run to start build ([Guide](/wiki/Crave_Devspace#building-using-crave-run-command))
- Pulling your Output ([Guide](/wiki/Crave_Devspace#pulling-output))
- Destroying old crave clone(optional) ([Guide](/wiki/Crave_Devspace#setting-up-the-project))

## Downloading

1. Now head on over to [foss.crave.io](https://Foss.crave.io)
and press the Downloads button in the Left Hand Side menu.

#### *Windows*

On windows, download the zip, extract it, enter it.

Right click somewhere blank on the folder and click on Open In Terminal.
Alternatively,

```
cd /d Drive:\path\to\crave\folder\ 
```

(replacing `Drive:\path\to\crave\folder\` with the full or relative path to your
crave folder)


Tip: If you use powershell and crave is not recognised but crave.exe exists in your current folder, you can use `./crave.exe` instead of `crave` to launch Devspace CLI. After that, go back to using `crave` when you enter Devspace CLI since it's a ubuntu environment with crave set up correctly.

#### *Linux*

- If you're on linux, run this command to download crave binary

```
curl -s https://raw.githubusercontent.com/accupara/crave/master/get_crave.sh | bash -s -- 
```

- Run these commands to install it systemwide

```
mkdir -p ${HOME}/bin/
```

```
mv ${PWD}/crave ${HOME}/bin/ 
```

```
sudo ln -sf /home/${USER}/bin/crave /usr/bin/crave; sudo chmod +x /usr/bin/crave 
```

#### *Mac*

- If you're on macOS, run this command to install crave

```
brew install --cask crave
```
You will need to [install homebrew](https://brew.sh/) beforehand.

<!--#### *Android*
If you're on Android, You will need to [install Termux first](https://github.com/termux/termux-app)

As of now, the crave binary doesn't work on termux.  
So, we will use `SEGFAULT` to use crave. (Read more about [segfault](http://thc.org/sf))

Here, We will be trying to cover these:
1. Connect to `segfault` via `ssh`.
2. Save the `first time login` info to your `termux environment`, so that you can always use the same instance.
3. Install crave.
4. At last, we will use `tmux`, as it's really useful (you should use it) Read the guide how to use `tmux` [here](<https://hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/>)

NOTE: It's necessary for you to ssh to your old segfault instance because if you start up a new instance every time, you will loose the files which you placed in your earlier instance.
Also, running more than one instance per user is considered 'ABUSE' by segfault.

So, Now, open termux and start copy pasting!

1. Connect to `segfault` via `ssh`:

- Run this command
```
ssh root@segfault.net
```
- then it will show you a big message (don't worry about it, also it doesn't needs any explanation), just type `yes` and press enter
- now it will ask for password, which is `segfault` (all lowercase), just type the password and press enter.

By now you will be in the segfault's instance.

NOTE: When you connect it to for the first time, you will see a big, colourful message, which have the useful things we will need later, so please don't clear it or exit the session.

2. Save the `first time login` info to your `termux environment`:

STEP 1:
- In our big, colourful message, you will see two `cat` commands & one `chmod` command
- you are needed to run these commands in termux and not in the segfault's instance (please don't get confused, after running that ssh command, the shell you are now looking at is segfault's)
- to open a new session of termux in current window, swipe from the left of the screen, and press new session
- now you can copy paste the things that were shown when you ssh'ed to segfault from your termux session

STEP 2: 
- After running those 3 commands, you are now good to go.
- out of those three, there was a cat command, which looked something like this
```
cat >>~/.ssh/config <<'__EOF__'
      host hostname      #(it's random)
      user root
      :
      :
__EOF__
```
From here we need 'hostname' (defiend in `host hostname` line) as you will need to write it everytime you want to ssh to your segfault instance.

Note: If it's too long, or you find it difficult to remember or hard/long to type, you simply run `nano ~/.ssh/config` and then edit it (warning: just edit the text after host, DO NOT TOUCH anything else there)

STEP 3:
- So, by now I think, you know your host's name, according to your SSH config.
- you can connect to your instance by typing this on Termux.
```
ssh hostname
```

Your segfault's setup is done, and you can install crave as usual.

Big Thanks to omansh-krishn for [this guide](https://github.com/omansh-krishn/crave/blob/main/crave.md) !-->

#### *Android*

[Here is a guide to use proot-distro on Android](/wiki/crave-install-guides/android)

After setting it up, [come back to this guide and continue](#setting-up-the-project)

#### *Web*

Open [sessions](https://foss.crave.io/app/#/session?team=14) tab and
click on "Create Session/Connect" button.

Now, [skip to the next
section](/wiki/Crave_Devspace#setting-up-the-project) without worrying about entering Devspace CLI.

For menu, swipe from the middle of the left side of your phone. If you're on a computer/have access to a keyboard, press `Ctrl+Alt+Shift`. This menu allows you to customize the terminal, and switch input method, like phone keyboard instead of emulated one. 

Crave RAS is built upon [Apache Guacamole](https://guacamole.apache.org/)

There's also a VSCode Web Client, instructions to set this up can be found [here](/wiki/Crave_Tricks#wiki/Crave_Tricks#vs-code-web)

### How to Prepare Environment

2. Now open up your browser again and navigate back to foss.crave.io
dashboard. On the same Left Hand Side menu, and go to API keys.

Create a new one, name it whatever you like to easily identify it and
click on Create API Key Button.

Now, download this newly created API key and copy it over to your crave
folder, wherever it is. On linux, preferably put it in your home
directory, or wherever your terminal leads you when you first open it.

3. Finally, to enter Devspace CLI: simply run

```
crave devspace
```

### Bonus: Using tmux

Using tmux is recommended to quickly view builds, multitask, etc. Here
is a simple guide on how to use tmux:

<https://hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/>

## Setting Up The Project

1. First use list to check the available projects:

```
crave clone list  # note down the project id for the next step
```

2. Then, use crave clone create for the project that you'd like to
build! This will automatically snapclone the latest source code for the
project that you are trying to build in a matter of seconds

```
crave clone create --projectID 72 /crave-devspaces/Lineage21
```

3. Remember to enter that folder you just created
```
cd /crave-devspaces/Lineage21
```

After you're done building the ROM at the end of this guide, use crave
clone destroy to delete the folder. Do not use rm -rf!

```
crave clone destroy /crave-devspaces/Lineage21
```

## Building Using Crave Run command

### How to build using Crave Run Command

Simply use crave run! On a normal server, we'd be running commands one
by one. But since crave uses a queue system, we run all the commands in
one go, and wait for our turn in queue. Then, the build node will
execute our commands, one by one.

- Syntax:

```
crave run --no-patch -- "your commands"
```

- If you'd like a clean build, add --clean flag

Note: using clean build will reset the image to default. This means it removes any of your progress e.g. synced dt/or the out folder from previous build and ensures we're back to the default source code of the base project. 

Please avoid doing this needlessly as resyncing and building from takes a lot of time. 

Syntax:

```
crave run --clean --no-patch -- "your commands"
```

When you run a build using crave run, it adds you to the build queue,
where a build node comes, picks it up and compiles your build for you!

### How to clone device sources inside Crave Run Command

Options: officially supported devices, local manifests, sync scripts,
and manual git clone.

- Officially Supported devices: They will automatically clone source
  code through breakfast or brunch commands, just refer to the official
  instructions for your device. You will need proprietary blobs, which
  can usually be found on TheMuppets repositories.

``` 
crave run  --no-patch -- "rm -rf .repo/local_manifests; \
git clone https://github.com/TheMuppets/manifests --depth 1 -b lineage-21.0 .repo/local_manifests; \
/opt/crave/resync.sh; \
source build/envsetup.sh; \
brunch Mi439"   
```

- Local manifests example:

```
crave run  --no-patch -- "rm -rf .repo/local_manifests; \
git clone https://github.com/sounddrill31/reponame --depth 1 -b branchname .repo/local_manifests; \
/opt/crave/resync.sh; \
source build/envsetup.sh; \
lunch lineage_oxygen-eng; \
m bacon"   
```

[~marado](https://tilde.pt/~marado/) made an easy to follow guide on
making your own local manifests over at
[tilde.pt](https://tilde.pt/~marado/blog/repo-using-a-local-manifest.html).

You can also use sounddrill's [Generator script](https://github.com/sounddrill31/actions_generate_local_manifests) which works most of the time 

Local manifests rely on repo sync. We have made a simple script to repo
sync while avoiding majority of conflicts which arise due to uncommitted
changes, or when building a different ROM. You can find the source code
to resync.sh
[here](https://github.com/accupara/docker-images/blob/master/aosp/common/resync.sh)

- Git clone example:

```
crave run  --no-patch -- "rm -rf device/oem/codename kernel/oem/codename vendor/oem/codename; \
git clone https://github.com/sounddrill31/android_device_oem_codename --depth 1 -b branchname device/oem/codename; \
git clone https://github.com/sounddrill31/android_kernel_oem_codename --depth 1 -b branchname kernel/oem/codename; \
git clone https://github.com/sounddrill31/android_vendor_oem_codename --depth 1 -b branchname vendor/oem/codename; \
source build/envsetup.sh; \
lunch lineage_oxygen-eng; \
m bacon"
```

## Bonus: Building Unsupported ROM
This is discouraged as it causes many sync conflicts and is a bit difficult to debug, however, it is not against the rules.
Rules for doing this:

- Do not use rm -rf * or cd into another folder in crave run before
  syncing, no matter who tells you to
- Fix sync conflicts instead of running the above commands
- (General Rule) Do not queue multiple builds at once
- Sync android 14 ROMs on Android 14 base project only

Example: Building crDroid 14

1. Set up closest cousin project that crave supports. Since it's
crDroid a14, you could use CipherOS or LineageOS for this (like
[this](/wiki/Crave_Devspace#setting-up-the-project))
using crave clone create.

2. Run crave run command, but reinit your preferred rom inside

```
crave run --no-patch -- "rm -rf .repo/local_manifests; \
repo init -u https://github.com/crdroidandroid/android.git -b 14.0 --git-lfs; \
git clone https://github.com/youraccount/local_manifests --depth 1 -b rising-14 .repo/local_manifests; \ 
/opt/crave/resync.sh; \
source build/envsetup.sh; \
breakfast device_codename userdebug; \ 
mka bacon"
```

## Pulling Output

### Automatic Method: Github Releases upload.sh script

- Enter the directory where you used crave run
- Create token.txt with your github PAT in there(ensure it has necessary
  permissions)
- Use the upload script

```
bash /opt/crave/github-actions/upload.sh 'tag' 'device' 'repository' 'release title' 'extra files'
```

(Replace tag with the tag you want your release to use, device with the
device's codename, repository with the link to the github repo and
release title with your preferred title. Extra files can be left blank)

You can find the source code for the script [here](https://github.com/accupara/docker-images/blob/master/aosp/common/upload.sh)

Tip: By default, size limit is set to 2147483648. To change it, run this
before the above command

```
export GH_UPLOAD_LIMIT="yourvalue"
```

### Automatic Method: Telegram upload.sh script

- Enter the directory where you used crave run
- [Set up
  telegram-upload](https://github.com/Nekmo/telegram-upload#-usage)
- Use the upload script

```
/opt/crave/telegram/upload.sh 'device' 'extra files'
```

Uploads will land in your saved messages. You can find the source code
for the script [here](https://github.com/accupara/docker-images/blob/master/aosp/common/tgup.sh)

Tip: By default, size limit is set to 2147483648. To change it, run this
before the above command

```
export TG_UPLOAD_LIMIT="yourvalue"
```

### Manual Method: How to pull output to Devspace CLI

- Simply use the crave pull command inside the same directory as before:

```
crave pull out/target/product/*/*.zip
```

```
crave pull out/target/product/*/*.img
```

- This will pull the built zip file to your devspace CLI from the build
  node.

<!-- -->

- From here, you can upload using github releases, [SourceForge](https://github.com/MaheshTechnicals/upload_to_sourceforge), telegram-upload, etc

# Advanced

### Bonus: Building without Devspace CLI

- Follow all the steps except do not enter into Devspace CLI(crave
  devspace command).
- Crave run, pull, etc should work on local machine as well(as long as
  crave is installed/set up along with a valid crave.conf)
- You might also want to install google's repo tool:

```
mkdir -p ~/.bin;
```
```
PATH="${HOME}/.bin:${PATH}";
```
```
curl https://storage.googleapis.com/git-repo-downloads/repo > ~/.bin/repo;
```
```
chmod a+rx ~/.bin/repo
```
**Do Not Do This Inside Crave Devspace CLI!**


### Setting up the Project - Alternative Method

Instead of setting up through crave clone create, you could directly repo init a supported ROM too! Crave will see git project is set up and assign accordingly. Note that this method is not recommended if you're using Devspace CLI or RAS(web client). This makes sense when you're trying to trigger without devspace

```
mkdir /crave-devspaces/Lineage21; cd /crave-devspaces/Lineage21
```

```
repo init -u https://github.com/LineageOS/android.git -b lineage-21.0 --git-lfs --depth=1
```

- At the time of writing, there was a known issue with LineageOS
  projects (Error: Found multiple matching projects, please specify
  project id on command line).
- If you're initializing lineageOS, use the following repos instead of
  LineageOS/android

For lineageOS 21, run

```
repo init -u https://github.com/LineageOS/android.git -b lineage-21.0 --git-lfs --depth=1
```

For lineageOS 20, run

```
repo init -u https://github.com/accupara/los20.git -b lineage-20.0 --git-lfs --depth=1
```

**Do not run repo sync or build the ROM here as it slows down devspace
CLI and causes trouble for other users. Devspace CLI also does not have
the resources to build a ROM or a similar huge project directly.**

### Workspace Persistence

Workspace persistence here refers to the preservation of what you have
synced and compiled in the past. This is not needed if you're building from Crave RAS(the web shell)

This build storage is reset when one of these 4 factors change:

```
1. Project UUID
2. User ID  
3. Workspace Dir 
4. Workspace Branch Sig 
```

- **Project UUID** is a unique ID to differentiate one project from
  another (this is automatically handled by crave and cannot be changed
  by users)
- **User ID** is a unique ID assigned to users (this is automatically
  handled by crave and cannot be changed by users)
- **Workspace Dir** is the name of the folder you have set up just
  now(the name/path matters, not the contents). Here are the recommended
  directory names if you frequently use my [github actions
  repo](https://github.com/sounddrill31/crave_aosp_builder):

```
/crave-devspaces/Arrow13
/crave-devspaces/Lineage20
/crave-devspaces/Lineage21 
/crave-devspaces/DerpFest13 
/crave-devspaces/Cipher14 
/crave-devspaces/Pixel14
/crave-devspaces/Lineage16
/crave-devspaces/Cyanogen14
```

Tip: If you find this part confusing, just ensure your workspace folder
is named as one of the folders listed above or the one you previously
built with

- **Workspace Branch Sig** is the Signature generated from the machine
  name. This changes if you trigger builds using different machines,
  often with different hostnames. To ignore this, you can use
  ignoreClientHostname inside crave.yaml as shown in the next section.

Tip: You use --clean flag in crave run to reset the build storage as
shown
[here](/wiki/Crave_Devspace#how-to-build-using-crave-run-command)

### Crave.yaml

This is a file to pass specific instructions to crave. Because repo init
put files in .repo/manifests, that's where our crave.yaml should go.
Move it to <Workspace Dir>/.repo/manifests/crave.yaml.

crave.yaml can be used to pass environment variables, push certain
files, use a certain docker image, etc.

The name has to match with the project name on dashboard(eg "Arrow OS"
"LOS 20"). You can also find this through crave clone list.

Examples:

```
CipherOS:
  ignoreClientHostname: true 
Arrow OS:
  ignoreClientHostname: true 
DerpFest-aosp:
  ignoreClientHostname: true 
LOS 20:
  ignoreClientHostname: true 
LOS 21:
  ignoreClientHostname: true
PixelOS:
  ignoreClientHostname: true
Rising OS:
  ignoreClientHostname: true
LOS 16:
  ignoreClientHostname: true
LOS CM 14.1:
  ignoreClientHostname: true  
```

```
TWRP:
  ignoreClientHostname: true
  image: "sounddrill31/crave-archlinux-twrp@sha256:605892162a907ea3760813643c9aa1bdb63a7ae0dce6ca159b0f6e20a7c0815b"
```

Do not change the image unless you know what you're doing and have a very good reason. 

For more info, read the [official
documentation](https://foss.crave.io/docs/crave-usage/#location-of-the-craveyaml-file)

Tip: If you find this part confusing, just run this command inside the
folder we made before

```
rm .repo/manifests/crave.yaml* || true; # Removes existing crave.yamls

curl -o .repo/manifests/crave.yaml https://raw.githubusercontent.com/sounddrill31/crave_aosp_builder/main/configs/crave/crave.yaml.aosp # Downloads crave.yaml
```

### Optional: Setting up build environment

Use this command to enter into a machine with your source code mounted:

```
crave ssh
```

This is good for changing files for the purpose of testing but this will
be wiped if you do a clean build.

**Do not run repo sync or build the ROM here, wait in the crave run
queue for a more powerful machine.**

You could get banned for syncing or building in this environment!

### Paid Queue
You can use crave tokens to skip the queue and get the build to start faster! To use tokens from your wallet, use the flag `--platform aosp-silver`

Like this:
```
crave run --no-patch --platform aosp-silver -- "build commands here"
```

If there are no tokens left in your wallet, the paid build will fail(but you can still use the free queue). A running build is able to go into Negative credits but no new paid builds can be requested while having less than 0 credits.

[Crave doc for Wallet](https://foss.crave.io/docs/wallets)
