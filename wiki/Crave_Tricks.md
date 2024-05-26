## Crave Tricks
### Tunneling through Devspace CLI
The crave client has a flag called link-services which allows you to hook-up a port to your host system

#### VS-Code Web
In this section, we'll be using code-server(preinstalled on devspace CLI) to start a session.

#### VNC

#### VS-Code Tunnel
This works well if you can access [vscode.dev](https://vscode.dev) but cannot use the crave binary.


#### Debugging
If things don't connect and you're sure that you did it right, try this:
- Stop your session from Sessions tab in dashboard
- Run the command again with link-services
- start a new tmux session with code-server or whatever you are running.

If you still can't get it to work, also try asking in the ext-devspace channel in the [discord server](https://discord.crave.io). 