# Rules
These are some rules users are expected to follow. Ignoring these rules may result in a temporary or permanent ban
## Devspace CLI Rules
- ***Do not use repo sync, use crave run instead***. If you try to install repo tool manually, it is looked at as an act to circumvent the rules and will likely result in a ban
- ***Be mindful of Devspace CLI Storage***. Clean up your .zip and .img build artefacts if you're used to pulling them to devspace
- ***Do not rm -rf a crave clone***. Use crave clone destroy path/to/folder/ instead. Use crave clone list to know the folder path.
- ***No building***: Do not try to build inside Decspace CLI using commands like make, mka, mm, m, etc. Use crave run instead.

## Queue Rules
Here are some simple Rules you are expected to follow to ensure everyone in the queue is happy:
- ***Do not use make clean or rm -rf out***. This slows down the queue because a 15 to 30 min incremental build becomes a full 3-4 hours build from scratch. Frequent use of this command without reason may cause temporary ban. 
- ***Do not abuse --clean needlessly***. Use only if there is a legitimate need and contact a queue moderator through the official chats when in doubt. This is done for the same reasons as the above rule.
- ***Do not make a folder and sync inside that to avoid conflicts*** (like `cd folder; repo sync`)
- ***Private repos***. Do not help users set up cloning from private repos because this goes against the "foss" in foss.crave.io.

## Community Guidelines
- ***Read the wiki, and make an effort to fix your problems before asking***. This gives you enough context to help us help you.
- ***Always provide all relevant info***. If it is a build error, for example, provide your error logs(preferably as a link to a paste service), your device specific sources, and enough context to explain the problem properly. 
- ***Report Crave Specific Errors*** in [Crave Errors Thread](https://discord.com/channels/709647870030250026/1194685316745924649)(you need to join the [discord server](https://discord.crave.io) first). Remember to eliminate all other possibilities and test things thoroughly as the team and community is trying their best to help us
