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
   <img src="https://github.com/slimreap3r/Writeups-n-Walkthroughs/blob/main/assets/opacity/opacity13.png" width="550" height="250">
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
    It seems that only <b>.png</b> files can be uploaded... time to do some research 🏃🏽‍♂️ 
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
    We now have a shell on the machine! 
</p>

<p align="center">
   <img src="https://github.com/slimreap3r/Writeups-n-Walkthroughs/blob/main/assets/opacity/opacity23.png" width="550" height="250">
</p>
<p align="center">
   <img src="https://github.com/slimreap3r/Writeups-n-Walkthroughs/blob/main/assets/opacity/opacity24.png" width="450" height="150">
</p>

<p align="center">
    A search of the only available user's files show an interesting one <em>dataset.kdbx</em> 
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
   <img src="https://github.com/slimreap3r/Writeups-n-Walkthroughs/blob/main/assets/opacity/ssh2sysadmin.png" width="550" height="250">
</p>

<p align="center">
    We now have a password for the <em>sysadmin</em> user which we can use to ssh into the machine 
</p>

## Privilege Escalation

<p align="center">
   <img src="https://github.com/slimreap3r/Writeups-n-Walkthroughs/blob/main/assets/opacity/opacity34.png" width="550" height="250">
</p>
<p align="center">
   <img src="https://github.com/slimreap3r/Writeups-n-Walkthroughs/blob/main/assets/opacity/opacity35.png" width="550" height="150">
</p>

<p align="center">
    Use <em>pspy64</em> to check for running processes and I discover a script that runs every minute.  
</p>

<p align="center">
    <em>script.php</em> also contains another script called <em>backup.inc.php</em> which is owned by sysadmin!  
</p>
<p align="center">
    The file cannot be written to so we must copy it to our home directory first and then move it to its original location.  
</p>

#### script.php
```php
<?php

//Backup of scripts sysadmin folder
require_once('lib/backup.inc.php');
zipData('/home/sysadmin/scripts', '/var/backups/backup.zip');
echo 'Successful', PHP_EOL;

//Files scheduled removal
$dir = "/var/www/html/cloud/images";
if(file_exists($dir)){
    $di = new RecursiveDirectoryIterator($dir, FilesystemIterator::SKIP_DOTS);
    $ri = new RecursiveIteratorIterator($di, RecursiveIteratorIterator::CHILD_FIRST);
    foreach ( $ri as $file ) {
        $file->isDir() ?  rmdir($file) : unlink($file);
    }
}
?>
```
<p align="center">
   <img src="https://github.com/slimreap3r/Writeups-n-Walkthroughs/blob/main/assets/opacity/opacity38.png" width="650" height="350">
</p>

#### netcat
```sh
└─$ nc -lvnp 9001               
listening on [any] 9001 ...
connect to [10.13.62.192] from (UNKNOWN) [10.10.246.153] 35438

whoami
root
id
uid=0(root) gid=0(root) groups=0(root)
ls
proof.txt
snap
```

<p align="center">
   <h1><em>We are now root 🥷🏽</em></h1>
</p>

