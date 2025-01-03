# Crave Devspace CLI
## Android(Awaiting Testing, edit this once you test it and fix inconsistencies)
### Installing on Termux

Guide to use termux's proot-distro for crave devspace:
Link to proot-distro: https://github.com/termux/proot-distro

### Step 0: Install termux
Get it from [PlayStore](https://play.google.com/store/apps/details?id=com.termux&hl=en_IN) or [F-Droid](https://f-droid.org/en/packages/com.termux/)
### Step 1: Install proot-distro
Direct Command:
```bash
pkg install proot-distro
```

Alternative:
```bash
pkg install git
git clone https://github.com/termux/proot-distro
cd proot-distro
```

### Step 2: Set Up a Container using Proot
Install Ubuntu:
```bash
pd install ubuntu-oldlts
```

Enter the Ubuntu Container:
```bash
pd sh ubuntu-oldlts
```

Prepare for crave:
```bash
apt update; apt install ssh rsync wget -y
```

Download Crave:
```bash
curl -s https://raw.githubusercontent.com/accupara/crave/refs/heads/master/get_crave.sh | bash -s --
```

Grant Permissions and Move it:
```bash
chmod +x crave; mv crave /usr/local/bin/
```

### Step 2.5: Set up Credentials
Now open up your browser and navigate back to foss.crave.io dashboard. On the same Left Hand Side menu, and go to API keys.
- Create a new one, name it whatever you like to easily identify it and
click on Create API Key Button.
- Copy it
- Create a new file in your current shell called crave.conf
    - Paste the API Key here
### Step 3: Connect
Once you take your valid crave.conf, just connect using `crave devspace` and return to the [previous guide](../Crave_Devspace.md#setting-up-the-project)

### Step 3.5: Quick Reconnection
To Quickly reconnect, just run:
```bash
pd sh ubuntu-oldlts -- crave devspace
```

### Notes: 
if you facing issues:
- try to exit from termux
- then re-login with: 
    `pd sh ubuntu-oldlts`
- then try again:
    `crave devspace`

Tested on `ubuntu-oldlts`, though your results may vary.

You can try other distro: 
`pd list`

Thanks to Ica for discovering this method and writing the guide that this was based on