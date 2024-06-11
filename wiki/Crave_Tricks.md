# Crave Tricks
## Tunneling through Devspace CLI
The crave client has a flag called link-services which allows you to hook-up a port to your host system

### VS-Code Web
In this section, we'll be using code-server(preinstalled on devspace CLI) to start a session. Most of these steps will be obsolete soon, when the crave team has this auto-execute when VSCode button is clicked on the dashboard. For now, try this:

#### Connect to Session
Now, connect to your devspace with this command

```crave devspace```
 
Alternatively, you can start a new session from Sessions tab and connect to it.

#### Configure Things

run this command to set up code-server on port 5899 

```
curl -O https://raw.githubusercontent.com/sounddrill31/crave_aosp_builder/main/scripts/code-server.sh; \
bash code-server.sh
```

This is a simple script to automatically fetch configuration and start the vscode service. If this isn't running in devspace CLI, it'll open a tmux where code-server starts at port 5899.

#### Connect
Connect to it through your browser

Go into Sessions tab and click on "Connect VSCode" button. 

### VS-Code Tunnel
This works well if you can access [vscode.dev](https://vscode.dev) but cannot use the crave binary, as a backup for when VSCode RAS doesn't work.

To set it up, run this command and follow further instructions as shown on the screen:
```curl https://raw.githubusercontent.com/sounddrill31/crave_aosp_builder/main/scripts/vscode-tunnel.sh | bash && tmux a -t codetunnel```

This command basically downloads the script from [here](https://github.com/sounddrill31/crave_aosp_builder/blob/main/scripts/vscode-tunnel.sh), and attaches to the tmux window called codetunnel

Now, follow further instructions on the screen. 

## Debugging
If things don't connect and you're sure that you did it right, try this:
- Stop your session from Sessions tab in dashboard
- Run the command again

If you still can't get it to work, also try asking in the ext-devspace channel in the [discord server](https://discord.crave.io). 
