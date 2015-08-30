Nginx PHP5 FPM Restart Fix on Ubuntu
====================================
[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/AbhishekGhosh/Nginx-PHP5-FPM-Restart-Fix-on-Ubuntu?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)

Fixes Ubuntu-Nginx Service php5-fpm restart issue. This is a known bug which has reappeared. If you run `tail -f /var/log/php5-fpm.log&` - you'll find nothing wrong. Do not stop / kill the process in production server. If it does not start, it will be worser! 

This conf needs edit :

`/etc/init/php-fpm.conf`

### Note for newer Ubuntu 14.04 LTS

Updated on [30th Aug, 2015] 

I found that, `/etc/init/php-fpm.conf` has been `/etc/init/php5-fpm.conf` now (HP Cloud, Ubuntu 14.04 LTS, Partner Image, also on Softlayer Ubuntu 14.05 LTS) ! In that case, `cd` to `/etc/init/` and do a `ls` to check the file name. Thing has not been corrected, only the filename can be different (php-fpm or php5-fpm). cd to ``/etc/init/` and find the file named `php-fpm.conf` or `php5-fpm.conf`, cat to see the content and find the lines :

````
# Precise upstart does not support reload signal, and thus rejects the
# job. We'd rather start the daemon, instead of forcing users to
# reboot https://bugs.launchpad.net/ubuntu/+source/php5/+bug/1272788
#
# reload signal USR2
````

and uncomment this :

`reload signal USR2`

on running `service php5-fpm restart`, you'll get warning though. Because, this script :

`/etc/init.d/php5-fpm`

has issues on Ubuntu 14.xy.

Open Filezilla, take backup of `/etc/init.d/php5-fpm` on your computer, delete `php5-fpm` script from `/etc/init.d/` and create a new script :

`nano /etc/init.d/php5-fpm`

You can check it :

````
cd /etc/init.d && ls
cat php5-fpm 
````

Copy this repo's the file named `php5-fpm`'s material full text as raw and wrtite out.
Save and run `service php5-fpm restart`. 
Change the ownership of the file to none or activate UNIX Wheel Group [see my details on wheel group - https://thecustomizewindows.com/2014/03/what-is-wheel-group-in-unix-unix-like-os/ ], 
otherwise on `dist-upgrade`, it can get replaced.

You can empty the `php5-fpm` file by :

````
cd /etc/init.d
cp /etc/init.d/php5-fpm ~/php5-fpm
echo " " > php5-fpm
````
and paste the content.

### Why it Happens?

By default it never happens. Possibly after optimizing the `www.conf` file, somehow it activate this.



### Wiki

We are trying to create a small wiki. You can help us to edit the wiki [here](https://github.com/AbhishekGhosh/Nginx-PHP5-FPM-Restart-Fix-on-Ubuntu/wiki).

### Others

1. My website will not load on Windozzz XP + IE ver 1 to 6, ver 8 ! Use a modern OS and browser. It is HSTS Preload listed. There is no matching cipher suite.
2. If you use HHVM with PHP5-FPM fallback, this file will not hamper your work. I will suggest to use HHVM with PHP5-FPM fallback now for WordPress for Faster loading. HHVM needs some tweaks, search on my website, you'll get some. Facebook fully copy-pasted many files and docs, the expire header need huge works. 
3. Nginx - PHP5-FPM does not purge due to other reason, Nginx Plus purges. I tested on 22nd Feb, 2015; for community edition, this change has nothing to to do with purging. 

### If you do not want to change the default file

If you want to fully remove (be careful, it will remove all the settings) and reinstall a fresh copy, this is the command :

````
sudo apt-get purge php5-fpm && sudo apt-get install php5-fpm

````

Initially `service php-fpm restart` will work.

This is fully Copyleft. You can verbatim copy, distribute without any credit. 

Also see https://github.com/AbhishekGhosh/Nginx-PHP5-FPM-UNIX-Socket-Configuration to fix latest Nginx PHP5-FPM UNIX Socket related issues.
