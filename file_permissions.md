## 1: set your user as the owner
```chown -R john /var/www/my-website.com/```

## 2:set the web server as the group owner
```chgrp -R www-data /var/www/my-website.com/```

## 3:750 permissions for everything
```chmod -R 750 /var/www/my-website.com/```
The third command sets the permissions: read, write and execute (7) for the owner (i.e. you), read and execute (5) for the group owner (i.e. the web server), zero permissions at all (0) for others. Once again this is done on every file and folder in the directory, recursively.

## 4:new files and folders inherit group ownership from the parent folder
```chmod g+s /var/www/my-website.com/```
The last command makes all files/folders created within the directory to automatically take on the group ownership of the parent folder, that is your web server. The s flags is a special mode that represents the setuid/setgid. In simple words, new files and directories created by the web server will have the same group ownership of my-website.com/ folder, which we set to www-data with the second command.

## 5:When the web server needs to write
If you have folders that need to be writable by the web server, you can just modify the permission values for the group owner so that www-data has write access. Run this command on each writable folder
```chmod g+w /var/www/my-website.com/<writable-folder>```
For security reasons apply this only where necessary and not on the whole website directory.