# Crave Tricks
## Tunneling through Devspace CLI
The crave client has a flag called link-services which allows you to hook-up a port to your host system

### VS-Code Web
In this section, we'll be using code-server(preinstalled on devspace CLI) to start a session.

#### Step 1
Run `code-server` to generate the config file

#### Step 2
Stop the command/exit out of the session

Go to the dashboard, Sessions Tab, and stop your session

#### Step 3
Now, connect to your devspace with this command

```crave devspace --link-service http:8080:8080```
 
Also run this command to turn off password authentication 

```sed -i.bak 's/auth: password/auth: none/' ~/.config/code-server/config.yaml```

(taken from official code-server docs)

Alternatively, you could set a password in ~/.config/code-server/config.yaml inside devspace

#### Step 4
Open a tmux and attach to it
```tmux```

(to reconnect to this, simply run `tmux a -t 0` since it is the first running tmux session)

#### Step 5
Run code-server
```code-server```

#### Step 6
Connect to it through your browser
http://localhost:8080

### VNC
Same steps as VS-Code Web except you don't need to run anything

#### Step 1
Kill old session(refer to step 2 of VS-Code Web section) and then connect to your devspace with this command:

```crave devspace --link-service vnc:5900:5900```

Check status of supervisord with this: 

```sudo service supervisor status```

If it says it is not running, start it using this command

```sudo service supervisor start```

Now, connect as per instructions [here](https://foss.crave.io/docs/devspaces/#vnc)


### VS-Code Tunnel
This works well if you can access [vscode.dev](https://vscode.dev) but cannot use the crave binary.

To set it up, run this command and follow further instructions as shown on the screen:
```curl https://raw.githubusercontent.com/sounddrill31/crave_aosp_builder/main/scripts/vscode-tunnel.sh | bash && tmux a -t codetunnel```

This command basically downloads the script from [here](https://github.com/sounddrill31/crave_aosp_builder/blob/main/scripts/vscode-tunnel.sh), and attaches to the tmux window called codetunnel

Now, follow further instructions on the screen. 


## Debugging
If things don't connect and you're sure that you did it right, try this:
- Stop your session from Sessions tab in dashboard
- Run the command again with link-services
- start a new tmux session with code-server or whatever you are running.

If you still can't get it to work, also try asking in the ext-devspace channel in the [discord server](https://discord.crave.io). 