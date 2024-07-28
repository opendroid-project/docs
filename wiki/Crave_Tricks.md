# Crave Tricks
## Tunneling through Devspace CLI
The crave client has a flag called link-services which allows you to hook-up a port to your host system

### VS-Code Web
In this section, we'll be using code-server(preinstalled on devspace CLI) to start a session.

#### Connect
Connect to it through your browser

Go into Sessions tab and click on "Connect VSCode" button. 

### VS-Code Tunnel
This works well if you can access [vscode.dev](https://vscode.dev) but cannot use the crave binary or the website for some reason, as a backup for when VSCode RAS doesn't work.

To set it up, run this command and follow further instructions as shown on the screen:
```curl https://raw.githubusercontent.com/sounddrill31/crave_aosp_builder/main/scripts/vscode-tunnel.sh | bash && tmux a -t codetunnel```

This command basically downloads the script from [here](https://github.com/sounddrill31/crave_aosp_builder/blob/main/scripts/vscode-tunnel.sh), and attaches to the tmux window called codetunnel

Now, follow further instructions on the screen. 

## Debugging
If things don't connect and you're sure that you did it right, try this:
- Stop your session from Sessions tab in dashboard
- Run the command again

If you still can't get it to work, also try asking in the ext-devspace channel in the [discord server](https://discord.crave.io). 
