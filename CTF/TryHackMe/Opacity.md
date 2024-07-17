# Opacity - Easy 

<p align="center">
   <img src="https://github.com/slimreap3r/Writeups-n-Walkthroughs/blob/main/assets/opacity/opacitytop.png" width="850" height="250">
</p>

#### This is a simple Linux machine with a PNG file upload vulnerability that can be exploited to gain a shell on the machine. After further enumeration of the machine, you can find a Keepass database file that belongs to another user on the machine. This file will contain the password for the *sysadmin* account. You can find the first flag, `local.txt`, in their /home directory. This account does not have root privileges so privilege escalation is needed. In order to get the final flag `proof.txt`.


## Enumeration 

<p align="center">
   <img src="https://github.com/slimreap3r/Writeups-n-Walkthroughs/blob/main/assets/opacity/opacity1.png" width="800" height="450">
</p>
