Nginx PHP5 FPM Restart Fix on Ubuntu
====================================

Fixes Ubuntu-Nginx Service php5-fpm restart issue. This is a known bug which has reappeared. If you run `tail -f /var/log/php5-fpm.log&` - you'll find nothing wrong. Do not stop / kill the process in production server. If it does not start, it will be worser! 

This conf needs edit :

`/etc/init/php-fpm.conf`

Update [28th Dec, 2014] : I found `/etc/init/php-fpm.conf` has been `/etc/init/php5-fpm.conf` now (HP Cloud, Ubuntu 14.04 LTS, Partner Image) ! In that case, `cd` to `/etc/init/` and do a `ls` to check the file name. Thing has not been corrected, only the filename can be different (php-fpm or php5-fpm).

and uncomment this :

`reload signal USR2`

on running `service php5-fpm restart`, you'll get warning though. Because, this script :

`/etc/init.d/php5-fpm`

has issues on Ubuntu 14.xy.

Open Filezilla, take backup of `/etc/init.d/php5-fpm` on your computer, delete `php5-fpm` script from `/etc/init.d/` and create a new script :

`nano /etc/init.d/php5-fpm`

Copy this repo's the file named `php5-fpm`'s material full text as raw and wrtite out.
Save and run `service php5-fpm restart`. 
Change the ownership of the file to none or activate UNIX Wheel Group [see my details on wheel group - https://thecustomizewindows.com/2014/03/what-is-wheel-group-in-unix-unix-like-os/ ], 
otherwise on `dist-upgrade`, it can get replaced.

My website will not load on Windozzz XP + IE ver 1 to 6, ver 8. Use a modern OS and browser. It is HSTS Preload listed.
