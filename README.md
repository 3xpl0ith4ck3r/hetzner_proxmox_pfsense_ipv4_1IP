<p align="center">
	<a href="https://www.hetzner.com/"><img alt="Hetzner Logo" height="" width="300" src="https://www.hetzner.com/themes/hetzner/images/logo/hetzner-logo.svg"></a>
    <br>
	<a href="https://www.proxmox.com"><img alt="Proxmox Logo" height="" width="300" src="https://www.proxmox.com/images/proxmox/Proxmox-logo.png"></a>
    <br>
	<a href="https://www.pfsense.org/"><img alt="pfSense Logo" height="" width="300" src="https://upload.wikimedia.org/wikipedia/commons/2/2a/PfSense_logo.svg"></a>

</p>

# Project Goal
This project shall help everyone who is seeking a good solution and guideline to setup Proxmox into a Hetzner root or bare metal server. Also a setup with pfSense will be described. All settings will be done with an focus to security and to have a solid solution.
In the world wide web exisiting many solutions and guidelines, but they are most outdated.

<p align="center">
	<a href="https://www.buymeacoffee.com/3xpl0ith4cr" target="_blank"><img src="http://public.jc21.com/github/by-me-a-coffee.png" alt="Buy Me A Coffee" style="height: 51px !important;width: 217px !important;" ></a>
</p>

## Scope
### Hetzner
- Part 1 - Setup from the scratch and setup through rescue mode
- Part 2 - Setup proxmox with help of scripts from <a href="https://github.com/CasCas2/proxmox-hetzner">https://github.com/CasCas2/proxmox-hetzner</a>
- Part 3 - Secure your base Proxmox setup
- Part 4 - Network Interface configuration for 1 IP address and IPv4

### Proxmox
- 
- 
- 

### pfSense
- 
- 



Let's start
# Hetzner setup
### Part 1
Log in into your Hetzner account and switch over to the **robot**
<img src="frontend/images/screenshots/Hetzner_Robot.png">

Goto the chapter **server**

<img src="frontend/images/screenshots/Hetzner_Robot_2.png">

and select your server. Afterwards you can go directly to your rescue tab.

<img src="frontend/images/screenshots/Hetzner_Selected_Server.png">

Select Linux as the Operating System and click on **Activate rescue system**
For further details of the capabilities of the Hetzner Robot and Rescue System please follow the documentation from Hetzner. [https://docs.hetzner.com/robot]

==Please use for all passwords an password manager==

After the activation of the rescue system you have 60 minutes to reboot your server.
Before you do this safe the shown credentials into your password manager.
E.g.: [https://vault.bitwarden.com/#/login](https://vault.bitwarden.com/#/login) or self-hosted alternative [https://github.com/vineethmn/vaultwarden-docker-compose](https://github.com/vineethmn/vaultwarden-docker-compose)
You will receive the user name ==root== and the new ==password== only working for the rescue system.

Switch to the tab ==Reset== and selecte the **Execute an automatic hardware reset**

#### Alternative if you have already access to your server via SSH you can normal reboot your server
`shutdown -r now`

Start directly after you logoff off your ssh session with an ping to your public IP address.

If you doesn't had a ssh connection just start an ping.

Use an terminal to logon with your ssh credentials.

If you receive an identification change, than you had a previous connection to your server and you must get rid of the existing fingerprint.
`ssh-keygen -f "/home/<user path>/.ssh/known_hosts" -R "xxx.xxx.xxx.xxx"`

Afterwards you can access normaly and accept the new ECDSA key fingerprint of your server.
You should see an standard welcome screen from Hetzner with some basic informations.

<img src="frontend/images/screenshots/Hetzner_Rescue_1.png">

### Part 2 - Installation of Proxmox using predefined scripts and the rescue system
Please use the repository of CasCas2 and the very well created scripts for the proxmox installation.
[https://github.com/CasCas2/proxmox-hetzner](https://github.com/CasCas2/proxmox-hetzner)
forked from [https://github.com/extremeshok/xshok-proxmox](https://github.com/extremeshok/xshok-proxmox)

```
git clone https://github.com/CasCas2/proxmox-hetzner.git
cd proxmox-hetzner/
ls -lah
chmod +x *.sh
ls -lah
```

after these commands you will se the content of the scripts and they are all executable.
This is a standard procedure. It is absolutely enough to make only the ==install-hetzner*.sh== executable.

==Please check the scope of the script before you use it. Learn it what stuff is done inside the scripts!==

With ```./install-hetzner-nvme.sh prox1```i will start my installation. This is based on the standard welcome screen and the ordered hard drives or NVME storage inside the server.
==You can also use an FQDN for your server if you now it already==

The script is installing proxmox with the same tools Hetzner gives you ```InstallImage```
But afterwards some Bugfixes and security setups are also running. So really, please check the scripts.

This will take some time. So get a coffee or two. up to the perfromance of your server it will last round about 5 - 10 minutes.

At the end you will see an ==Installation complete== and that you can reboot.
`shutdown -r now` will help. But keep in mind that after the reboot an other ECDSA key fingerprint will be set.
So please use the time of the reboot and remove the fingerprint of the rescue system.
`ssh-keygen -f "/home/<user path>/.ssh/known_hosts" -R "xxx.xxx.xxx.xxx"`

Let a ping run. After the system is reachable you can connect with your root account and the same credentials as the rescue system has give you.

If everything has worked so far, you will see the default ssh response with your hostname.
<img src="frontend/images/screenshots/Proxmox_first_SSH.png">

We check if proxmox is running properly with `netstat - tunlp`
We searching for the port **22** (SSH) and **8006** (GUI from Proxmox Proxy)
<img src="frontend/images/screenshots/Proxmox_Check_netstat.png">

Please go to Part 3.



### Part 2 (alternative way) - Installation with standard Hetzner Installimage Script and selection of Proxmox
### Part 2 (pure alternative way) - Installation of blank Debian 11 and installation of Proxmox from APT
- [ ] ToDo: Describe or Link the default Hetzner way
- [ ] ToDo: Describe or Link the default Debian 11 with APT way

### Part 3 - Secure your fresh Proxmox Server
Based on the good post of TechnoTim and his "*Before I do anything on Linux, I do these 13 things*"
[https://docs.technotim.live/posts/fist-13-things-linux/](https://docs.technotim.live/posts/fist-13-things-linux/)

**But** based on the script the system is pure and many times updated.
Also from my perspective I don't like to have a productive system with unattended-upgrades. For this a like to setup solutions with Ansible.
- [ ] Describe and Link to Ansible Docker solution

#### Account
##### Add user 
It is very important to setup a separate user account and not use root for all stuff.
In this guide i will use ==user== as the username.
`adduser user` - please replace *user* with your username you want to have.

##### add to sudoer
`usermod -aG sudo user`

##### SSH Server
[https://linuxhint.com/use-ssh-copy-id-command/](https://linuxhint.com/use-ssh-copy-id-command/)
[https://phoenixnap.com/kb/linux-ssh-security](https://phoenixnap.com/kb/linux-ssh-security)

The target now is to copy sour ssh credentials to your server and switch to key based authentification.
Please remember that this client you are working will be the only one whith access to the server.
Before we start check if ssh is properly installed on your local system.
Execute`ssh` and `ssh-copy-id` without any parameter to check if this tools are installed.

Build your ssh-keygen with `ssh-keygen`
If your target will be only one client shall be possible to access the server via ssh than keep the filename as it is.
If not than change it to the client name. In my case **mx_client_id_rsa**
After the keygen is done you will find two files. Please be shure you don't give your private key outside of your hands. For this we use the `ssh-copy-id` tool.

`ssh-copy-id -i ~/.ssh/mx_client_id_rsa.pub user@144.76.69.153`

Please check directly afterwards with a new terminal that the credentials are working and you can connect to the server.

Check with `cat .ssh/authorized_keys` that only **one** key is available.
If you add further key's, please check after copy that only the amoung you want are available.
Logout from your user account ssh session.

Execute `nano /etc/ssh/sshd_config`and **add** the following parts.
```
ChallengeResponseAuthentication no
```

Please **change** in the same file the following parts.
Change the `#PasswordAuthentication yes` to `PasswordAuthentication no`
Change the `Port 22` to some other number `Port 2222` or `Port 22222` or anything you want.
Change the `PermitRootLogin yes` to `PermitRootLogin no`. Afterwards no login for root via ssh is possible.

Open a new ssh connection with your user account. ==If you renamed the file be sure to use the -i parameter.==
With the user account you can execute `sudo service ssh restart`.
Logout from the user account and re-login. If this works, please close your root ssh connection and try to re-login.
You should receive these message.
<img src="frontend/images/screenshots/Proxmox_SSH_secure.png">
This is a good point. Standard port will be refused and if you know the right port you need the correct publickey **and** the correct password of the key.

If you want further hardening for your server you can implement port knocking.
[https://www.howtogeek.com/442733/how-to-use-port-knocking-on-linux-and-why-you-shouldnt/](https://www.howtogeek.com/442733/how-to-use-port-knocking-on-linux-and-why-you-shouldnt/)

You can think about port knocking like it was in the 1920 prohibition time. You had access only if you know how to knock a specific door. *Funny time*
The same principle you can setup to your server. The articel from How-to Geek is very good and also explains in a good manner why you shouldn't use it.
To have a extreme variant of differnt ports you *knock knock* in sequence and after the correct sequence the hidden known port will be opened is really cool.
But if the daemon in the background is not working, your port will never be available.
My recommendation. If you want have such a secure configuration you need a second way of access to the server.
A fixed VNC connection to an jumphost (iptables) inside the LAN network. Only reachable through a VPN connection. In this case a port knocking from the WAN direction is possible!










# References
https://phoenixnap.com/kb/linux-ssh-security
https://github.com/CasCas2/proxmox-hetzner
https://schroederdennis.de/allgemein/proxmox-auf-rootserver-mit-nur-1-public-ip-addresse-pfsense-nat/
https://forum.proxmox.com/threads/hetzner-host-with-pfsense-and-additional-ipv4-address-slow-internet.74306/

https://www.youtube.com/playlist?list=PLNmsVeXQZj7oAAFVjNrAz1uRqIcanXreg
https://www.youtube.com/playlist?list=PLcxL7iznHgfXoNOuBqzzni4p_PgM_s71f
https://www.youtube.com/playlist?list=PLcxL7iznHgfVclnHZfar05VJ7IdsY4yx_
https://www.youtube.com/playlist?list=PLcxL7iznHgfVLL50jvHM43ZV4ru8N5Vab

https://forum.proxmox.com/threads/hetzner-proxmox-pfsense.54068/

https://docs.technotim.live/posts/fist-13-things-linux/
https://docs.technotim.live/posts/first-11-things-proxmox/



