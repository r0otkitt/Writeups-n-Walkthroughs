# Opacity - Easy 

<p align="center">
   <img src="https://github.com/slimreap3r/Writeups-n-Walkthroughs/blob/main/assets/opacity/opacitytop.png" width="850" height="250">
</p>

#### This is a simple Linux machine with a PNG file upload vulnerability that can be exploited to gain a shell on the machine. After further enumeration of the machine, you can find a Keepass database file that belongs to another user on the machine. This file will contain the password for the *sysadmin* account. You can find the first flag, `local.txt`, in their /home directory. This account does not have root privileges so privilege escalation is needed. In order to get the final flag `proof.txt`.


## Enumeration 

<p align="center">
   <img src="https://github.com/slimreap3r/Writeups-n-Walkthroughs/blob/main/assets/opacity/opacity1.png" width="550" height="550">
</p>

<p align="center">
   Initial scan shows several ports open. Lets see what directories are avaiable to us.
</p>

<p align="center">
   <img src="https://github.com/slimreap3r/Writeups-n-Walkthroughs/blob/main/assets/opacity/opacity8.png" width="550" height="250">
</p>

<p align="center">
   No easy way in with smb :(
</p>

<p align="center">
   <img src="https://github.com/slimreap3r/Writeups-n-Walkthroughs/blob/main/assets/opacity/opacity2.png" width="550" height="550">
</p>

<p align="center">
   <b>/cloud</b> seems interesting!
</p>

<p align="center">
   <img src="https://github.com/slimreap3r/Writeups-n-Walkthroughs/blob/main/assets/opacity/opacity4.png" width="550" height="550">
</p>

<p align="center">
   <img src="https://github.com/slimreap3r/Writeups-n-Walkthroughs/blob/main/assets/opacity/opacity5.png" width="550" height="550">
</p>

<p align="center">
   Two directories available. The login page does not accept the usual default credentials and at this point still do not know any users to try.
The File upload seems to only accept .png files. Trying to capture the request and repeat it through Burp yields no bypass because the file will not be uploaded. Must be some sort of server-side filter. 
</p>

<p align="center">
   <img src="https://github.com/slimreap3r/Writeups-n-Walkthroughs/blob/main/assets/opacity/opacity13.png" width="550" height="550">
</p>

<p align="center">
   Using <b>enum4linux</b> for some automated enumeration of the machine but this fails to yield any viable results to use.
</p>

<p align="center">
   <img src="https://github.com/slimreap3r/Writeups-n-Walkthroughs/blob/main/assets/opacity/opacity11.png" width="550" height="250">
</p>

<p align="center">
   Copy a rev_shell to use into my working directory. 
</p>

<p align="center">
   <img src="https://github.com/slimreap3r/Writeups-n-Walkthroughs/blob/main/assets/opacity/opacity14.png" width="550" height="550">
</p>

<p align="center">
   It seems that only `.png` files can be uploaded... time to do some research 🏃🏽‍♂️
</p>

<p align="center">
   <img src="https://github.com/slimreap3r/Writeups-n-Walkthroughs/blob/main/assets/opacity/opacity15.png" width="550" height="250">
</p>

<p align="center">
   A quick google search gives me the information needed. A file can be uploaded with a <b>#</b> before the .png!!!
</p>

<p align="center">
   <img src="https://github.com/slimreap3r/Writeups-n-Walkthroughs/blob/main/assets/opacity/opacity16.png" width="550" height="250">
</p>

<p align="center">
   Upload success!!!
</p>

## Initial Access

<p align="center">
   <img src="https://github.com/slimreap3r/Writeups-n-Walkthroughs/blob/main/assets/opacity/opacity17.png" width="550" height="250">
</p>

<p align="center">
   We now have a shell on the machine
</p>

<p align="center">
   <img src="https://github.com/slimreap3r/Writeups-n-Walkthroughs/blob/main/assets/opacity/opacity23.png" width="550" height="250">
</p>
<p align="center">
   <img src="https://github.com/slimreap3r/Writeups-n-Walkthroughs/blob/main/assets/opacity/opacity24.png" width="250" height="150">
</p>

<p align="center">
   A search of the only avaiable user's files show an interesting one <em>dataset.kdbx</em>
</p>

<p align="center">
   <img src="https://github.com/slimreap3r/Writeups-n-Walkthroughs/blob/main/assets/opacity/opacity28.png" width="550" height="80">
</p>
</p><p align="center">
   <img src="https://github.com/slimreap3r/Writeups-n-Walkthroughs/blob/main/assets/opacity/opacity29.png" width="350" height="250">
</p>

<p align="center">
   Using John and hashcat, we are able to crack the master password for the Keepass password manager. 
</p>

<p align="center">
   <img src="https://github.com/slimreap3r/Writeups-n-Walkthroughs/blob/main/assets/opacity/opacity32.png" width="550" height="250">
</p>

<p align="center">
   Now have a password for the <em>sysadmin</em> user
</p>

