# PHP Setup In Linux
 This is set up guide for WEB DEVELOPEMENT!
 
# Platforms
- Android (Using Termux Application)
- Ubuntu 20 LTS

# Table Of Contents
## Andriod
1. [PHP Setup](#php-a)
1. [MYSQL Setup](#mysql-a)
1. [CREATE MYSQL PRIVILIAGE](#mysql-priv)

# Andriod Platform

 Install Termux From Google Play Store or F-Droid first.
 (Seyup in Termux is PHP-7.4)
 
1. Fresher Installtion

```bash
apt update && apt upgrade -y
```

2. Upgrade All Packages

```bash
pkg up

```
3. <a name="php-a">Install PHP </a>

```bash
pkg install php
pkg install php-apache
```

4. Poof For PHP

```bash
php -v 
```
5. Install Apache2

```bash
pkg install apache2
```

6. Go To `/data/data/com.termux/files/usr`

```bash
cd $PREFIX
```

7. Go To Under Apache2 Dir

```bash
cd apache2
```

8. Edit `httpd.conf`

```bash
pkg install nano #to install code editor
nano httpd.conf
```
9. Added PHP7 Module at the end `httpd.conf`

```conf
</IfModule>

LoadModule php7_module libexec/apache2/libphp7.so
<FilesMatch \.php$>
   SetHandler application/x-httpd-php
</FilesMatch>
<IfModule dir_module>
    DirectoryIndex index.php
</IfModule>
```

10. Find `worker` Module of PHP

<kbd>CTRL</kbd>+<kbd>w</kbd> and type `worker` and <kbd>Enter</kbd> and you will see lile below : 

```conf
LoadModule mpm_worker_module libexec/apache2/mod_mpm_worker.so
```
and comment it!Like below : 

```conf
#LoadModule mpm_worker_module libexec/apache2/mod_mpm_worker.so
```

11. Find `prefork` Module for PHP7 to load PHP7 by [Arch Linux Wiki Page](https://wiki.archlinux.org/index.php/Apache_HTTP_server#PHP)
You will see like :

```conf
#LoadModule mpm_prefork_module libexec/apache2/mod_mpm_prefork.so
```

and Uncomment it

```conf
LoadModule mpm_prefork_module libexec/apache2/mod_mpm_prefork.so
```
and start apache server

```bash
httpd -v # Proof For Server 
killall httpd; httpd # restart server
apachectl start # to start apache server
apachectl stop #to stop server
apachectl restart #to restart when edited config files
```
Note: If you get servername error open `httpd.conf` file and search `ServerName` and edit loke below :

```bash
ServerName localhost:8080
```

11. Open Browser and search url `localhost:8080` you will see `It work!` that is default!
12. We will cchange Directory of DocumentRoot

13. Stop Server first  `apachectl stop`

14. If you are at `/home` you can type line 6,7,8 again.
15. Search `DocumentRoot` and change default to following

```bash
DocumentRoot "/sdcard/htdocs"
<Directory "/sdcard/htdocs">
```

16. Create Dir under Internal Storage

```bash
cd
cd /sdcard/
mkdir htdocs
```

17. Open htdocs from text editor!
18. Create php file and test it.
19. For Testing!

```bash
#under /sdcard/htdocs/
echo "<?php echo 'Hello World!' ?>" >>> index.php 
```

20.Start apache server `apachectl start` and open browser and check it work or not!

Note: You can change DocumentRoot directory what you like but don't add `/` end of dir root that will crash your server!!!

21. Enable apache rewrite module by searching `rewrite` at `httpd.conf` and uncomment it!

```conf
LoadModule rewrite_module libexec/apache2/mod_rewrite.so
```

# <a name="mysql-a">Install Mysql Server (Mariadb In Termux)</a>

1. Install Mariadb

```bash
pkg install mariadb
mysql # Of You get errors
```
like
```bash
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/data/data/com.termux/files/usr/tmp/mysqld.sock' (2)
```

You can start mysql server manually by opening `mysql.sh`.

2. Go to
```bash
cd
cd $PREFIX
cd etc
cd init.d
ls #you will see `mysql` file that is executable binary file!
```
3. Start Server like

```bash
./mysql status #check status running or not
./mysql start # start server
mysql #you will login into mariadb
```
like

```bash
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 4
Server version: 10.5.8-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]>
```
You are now loged in!
<kbd>CTRL</kbd>+<kbd>D</kbd> to exit! You can also type `exit` .
I prefer CLI :3

When you stop like `./mysql stop` you will get errors like

```bash
ERROR! MariaDB server process #4215 is not running!
```

You can see all process of mysql run  :
```bash
find 'etc' | grep 'mysql'
```
Stop process by running :

```bash
stop mysql
```
if not work

```bash
kill $(ps aux | grep '[m]ysql' | awk '{print $2}')
```
Note : If you want to stop other process change `[m]sql` to `[p]hp` if PHP! by [Easy Engine](https://easyengine.io/tutorials/linux/kill-all-processes/?amp)

and If you start again and by stopping `stop mysql` you need to run `mysqld_safe` .
Open new session and type `mysql` for Mysql CLI.

# <a name="mysql-priv">Create MYSQL PRIVILIAGE</a>

This setup is to access database from PHP 'username'.

# Ubuntu Linux
Installtion should done it yourself!

## UBTNU Installtion In Termux (skip on linux)

Create new file `ubuntu.sh` and  `grant it` by `chmod +x ubtnu.sh
Copy following and run `./ubuntu.sh` and wait some mimute. ⏱️
When complete  run `./ubuntu.sh` and autologin to ubuntu like :

```bash
root@localhost:~# #Example When Loged In
```
run 

```bash
pkg install wget openssl-tool proot -y && hash -r && wget https://raw.githubusercontent.com/EXALAB/AnLinux-Resources/master/Scripts/Installer/Ubuntu/ubuntu.sh && bash ubuntu.sh
./start-ubuntu.sh
```

or

Copy Below and run:
```bash
#!/data/data/com.termux/files/usr/bin/bash
folder=ubuntu-fs
if [ -d "$folder" ]; then
	first=1
	echo "skipping downloading"
fi
tarball="ubuntu-rootfs.tar.xz"
if [ "$first" != 1 ];then
	if [ ! -f $tarball ]; then
		echo "Download Rootfs, this may take a while base on your internet speed."
		case `dpkg --print-architecture` in
		aarch64)
			archurl="arm64" ;;
		arm)
			archurl="armhf" ;;
		amd64)
			archurl="amd64" ;;
		x86_64)
			archurl="amd64" ;;	
		i*86)
			archurl="i386" ;;
		x86)
			archurl="i386" ;;
		*)
			echo "unknown architecture"; exit 1 ;;
		esac
		wget "https://raw.githubusercontent.com/EXALAB/AnLinux-Resources/master/Rootfs/Ubuntu/${archurl}/ubuntu-rootfs-${archurl}.tar.xz" -O $tarball
	fi
	cur=`pwd`
	mkdir -p "$folder"
	cd "$folder"
	echo "Decompressing Rootfs, please be patient."
	proot --link2symlink tar -xJf ${cur}/${tarball}||:
	cd "$cur"
fi
mkdir -p ubuntu-binds
bin=start-ubuntu.sh
echo "writing launch script"
cat > $bin <<- EOM
#!/bin/bash
cd \$(dirname \$0)
## unset LD_PRELOAD in case termux-exec is installed
unset LD_PRELOAD
command="proot"
command+=" --link2symlink"
command+=" -0"
command+=" -r $folder"
if [ -n "\$(ls -A ubuntu-binds)" ]; then
    for f in ubuntu-binds/* ;do
      . \$f
    done
fi
command+=" -b /dev"
command+=" -b /proc"
command+=" -b ubuntu-fs/root:/dev/shm"
## uncomment the following line to have access to the home directory of termux
#command+=" -b /data/data/com.termux/files/home:/root"
## uncomment the following line to mount /sdcard directly to / 
#command+=" -b /sdcard"
command+=" -w /root"
command+=" /usr/bin/env -i"
command+=" HOME=/root"
command+=" PATH=/usr/local/sbin:/usr/local/bin:/bin:/usr/bin:/sbin:/usr/sbin:/usr/games:/usr/local/games"
command+=" TERM=\$TERM"
command+=" LANG=C.UTF-8"
command+=" /bin/bash --login"
com="\$@"
if [ -z "\$1" ];then
    exec \$command
else
    \$command -c "\$com"
fi
EOM

echo "fixing shebang of $bin"
termux-fix-shebang $bin
echo "making $bin executable"
chmod +x $bin
echo "removing image for some space"
rm $tarball
echo "You can now launch Ubuntu with the ./${bin} script"
```

Note: If you crash on Android google it!

Note: PHP8 is not avaliable for current Termux.

# PHP8.0 In Ubuntu 20
(also avaliable at android Ubuntu)

1. List existing PHP packages

```bash
dpkg -l | grep php | tee packages.txt
```

2. Add ondrej/php PPA

```bash
apt-get install sudo
sudo apt install software-properties-common
sudo add-apt-repository ppa:ondrej/php
sudo apt update
```
Steps above will add the PPA as a source of packages, that contains all PHP packages and their dependencies such as argon2 and libzip.

3. Install PHP 8.0 and extensions

All PHP 8.0 packages follow php8.0-NAME pattern, and php8.0-common package includes a sensible set default of extensions (such as `php8.0-).

```bash
sudo apt install php8.0
#sudo apt install php8.0-common php8.0-cli -y
```
 
Proof of PHP8 run  :

```bash
php -v # Show PHP version.
php -m # Show PHP modules loaded.
```
4. Add Afditional Extension

```bash
sudo apt install php8.0-{bz2,curl,intl,mysql,readline,xml}
```

5. Restart Apache Server

```bash
sudo service apache2 restart
```

6. Remove all Old Version of PHP

```bash
sudo apt purge '^php7.4.*'
```

# Apache2 80 Host Problem In Ubuntu (Sloved!)

1. To apache2 run :

```bash
cd /etc/apache2
```

2. Edit `ports.conf`

from
```conf
# If you just change the port or add more ports here, you will likely>
# have to change the VirtualHost statement in
# /etc/apache2/sites-enabled/000-default.conf

Listen 80

<IfModule ssl_module>
        Listen 443
</IfModule>

<IfModule mod_gnutls.c>
        Listen 443
</IfModule>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```
to
```conf
# If you just change the port or add more ports here, you will likely>
# have to change the VirtualHost statement in
# /etc/apache2/sites-enabled/000-default.conf

Listen 8080

<IfModule ssl_module>
        Listen 443
</IfModule>

<IfModule mod_gnutls.c>
        Listen 443
</IfModule>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```
3. Save and then run :

```bash
cd /etc/apache2/sites-enabled
```
4. Open `000-default.conf`

from
```conf
VirtualHost *:80>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to                  # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
       # ServerName localhost
        ServerAdmin webmaster@localhost                                                  DocumentRoot /var/www/
        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.                                                            # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
</VirtualHost>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```

TO
```conf
VirtualHost *:8080>
        # The ServerName directive sets the request scheme, hostname and port that
        # the server uses to identify itself. This is used when creating
        # redirection URLs. In the context of virtual hosts, the ServerName
        # specifies what hostname must appear in the request's Host: header to                  # match this virtual host. For the default virtual host (this file) this
        # value is not decisive as it is used as a last resort host regardless.
        # However, you must set it for any further virtual host explicitly.
        ServerName localhost
        ServerAdmin webmaster@localhost                                                         DocumentRoot /var/www/
        # Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
        # error, crit, alert, emerg.                                                            # It is also possible to configure the loglevel for particular
        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available/, which are
        # enabled or disabled at a global level, it is possible to
        # include a line for only one particular virtual host. For example the
        # following line enables the CGI configuration for this host only
        # after it has been globally disabled with "a2disconf".
        #Include conf-available/serve-cgi-bin.conf
</VirtualHost>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```
And create php file at 
`cd /var/www`
and 
create `index.php` and test code write and start `apache server`

```bash
sudo service apache2 restart
```
and Open in browser and check at `localhost:8080`.

# Setup Apache Server In Ubuntu

Search `var/www/html` and and change TO

```conf
<Directory /var/www/>                            Options Indexes FollowSymLinks           AllowOverride None                       Require all granted              </Directory>
```

add Servername at the end 

```conf
ServerName localhost
```
and go to `/etc/apache2/sites-enabled` Edit `000-default.conf` 

```conf
<VirtualHost *:8080>
        # The ServerName directive sets the request scheme>
        # the server uses to identify itself. This is used>
        # redirection URLs. In the context of virtual host>
        # specifies what hostname must appear in the reque>
        # match this virtual host. For the default virtual>
        # value is not decisive as it is used as a last re>
        # However, you must set it for any further virtual>
        ServerName localhost
        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/
        # Available loglevels: trace8, ..., trace1, debug,>
        # error, crit, alert, emerg.
        # It is also possible to configure the loglevel fo>        # modules, e.g.
        #LogLevel info ssl:warn

        ErrorLog ${APACHE_LOG_DIR}/error.log                       CustomLog ${APACHE_LOG_DIR}/access.log combined

        # For most configuration files from conf-available>
        # enabled or disabled at a global level, it is pos>
        # include a line for only one particular virtual h>
        # following line enables the CGI configuration for>
        # after it has been globally disabled with "a2disc>
        #Include conf-available/serve-cgi-bin.conf
</VirtualHost>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```
also change `ports.conf`
```conf
Listen 8080                                 
<IfModule ssl_module>
        Listen 443                          </IfModule>

<IfModule mod_gnutls.c>
        Listen 443                          </IfModule>
```

----
## Android and Linux (Ubuntu 20)

1. Start MySQL Server

```bash
#Android
mysqld_safe 
#Ubuntu
sudo service mysql start
```
and then run
```bash
mysql
# Output
# MariaDB [(none)]> SQL Here
```
2. Type SQL
```sql
CREATE USER 'root' IDENTIFIED BY '';
GRANT ALL ON db.* TO root@localhost IDENTIFIED BY '';
FLUSH PRIVILEGES;
```
Note:SQL statement is end with `;`

Now you can access by `root` username and password is ` ` empty on `port:3306` (default) of mysql.