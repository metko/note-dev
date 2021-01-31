# How to change php version with PHPBrew

## 1 - Install apache2 and phpbrew

- Install first apache2 + apache2-dev 
- Install PHPBrew
- Install a phpversion with phpbrew with commons variants

```
phpbrew install 7.X +default +apxs2 +mysql +sqlite +small +mb +zip +intl +xml +curl +gd
phpbrew variants # so see the list
```
Then swith the php version 
```
phpbrew switch php7.X
```

Notes:

To install gd, add an extension globally to phpbrew (work on ubuntu 20.04)
```
phpbrew -d ext install gd --
--with-libdir=lib/x86_64-linux-gnu
--with-gd=shared
--enable-gd-native-ttf
--with-jpeg-dir=/usr
--with-png-dir=/usr
--with-freetype-dir=/usr/include/freetype2/ft2build.h
--with-xpm-dir=/usr
--with-webp-dir=/usr
```

## 2 - enabled php execution in apache2.conf
```
<FilesMatch ".+\.ph(ar|p|tml)$">
    SetHandler application/x-httpd-php
</FilesMatch>
<FilesMatch ".+\.phps$">
    SetHandler application/x-httpd-php-source
    # Deny access to raw php sources by default
    # To re-enable it's recommended to enable access to the files
    # only in specific virtual host or directory
    Require all denied
</FilesMatch>
# Deny access to files without filename (e.g. '.php')
<FilesMatch "^\.ph(ar|p|ps|tml)$">
    Require all denied
</FilesMatch>

# Running PHP scripts in user directories is disabled by default
# 
# To re-enable PHP in user directories comment the following lines
# (from <IfModule ...> to </IfModule>.) Do NOT set it to On as it
# prevents .htaccess files from disabling it.
<IfModule mod_userdir.c>
    <Directory /home/*/public_html>
        php_admin_flag engine Off
    </Directory>
</IfModule>
```

Then restart apache2
```
sudo services apache2 restart
```

Maybe you will need to change the lib .so to corresponding phpversion in 
```/etc/apache2/modules-enabled/php7.load => Loadmodule php7 /usr/lib/apache2/modules/phplibxxxx.so```
Then restart apache2

