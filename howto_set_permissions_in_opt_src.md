# /opt/src folder permissions

As a convention:

- all folders in `/opt/src/` need to be owner-group `src` (the user-owner can be anybody in the `src` group).
- the group need to have `rw` (read-write) permission for files and `rwx` for directories.

When we create a new folder or file, it will have user and group owner set to the current user,
which is wrong because other users will not be able to edit these new files/folders.

To fix this follow he following steps every time a new folder is added to `/opt/src`.

When creating a new folder in `/opt/src`:

1. change the group-owner to `src`:

```
$ chgrp src multispot_utils -R
```

2. set the "getgid bit" permission, this will assure that new files and subfolder
   inherit the group `src` and permissions of parent folder:

```
chmod g+s multispot_utils
```

3. Check that permissions are right. The folder permission should look like this:

```
anto@geppetto:~/opt/src$ ll -d multispot_utils
drwxrwsr-x 4 anto src 4096 Jan 19 09:35 multispot_utils/
anto@geppetto:~/opt/src$
```

And the folder content permission should look like this:

```
anto@geppetto:~/opt/src$ cd multispot_utils/
anto@geppetto:~/opt/src/multispot_utils$ ll
total 32
drwxrwsr-x  4 anto src 4096 Jan 19 09:35 ./
drwxrwxr-x 11 root src 4096 Jan 19 09:35 ../
drwxrwxr-x  8 anto src 4096 Jan 19 09:35 .git/
-rw-rw-r--  1 anto src   64 Jan 19 09:35 .gitignore
-rw-rw-r--  1 anto src 1139 Jan 19 09:35 LICENSE
drwxrwxr-x  2 anto src 4096 Jan 19 09:35 multispot_utils/
-rw-rw-r--  1 anto src  264 Jan 19 09:35 README.md
-rw-rw-r--  1 anto src 1013 Jan 19 09:35 setup.py
anto@geppetto:~/opt/src/multispot_utils$
```


