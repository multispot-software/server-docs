# Linux Server Configuration


This documents describe the configuration of the Linux Server
that mounts shared folders from a Windows machine.

## Check network

Check that the windows machine can be reached from the linux one:

```
ping laser2002j-pc.ad.chem.ucla.edu
```

## Mount windows share from linux

Sometimes the share get stuck from the Windows side.
Rebooting the windows computer may fix the problem.
Sometimes if windows is rebooted for other reasons, the
folders are in a broken state and must be remounted.

Connect to geppetto with putty. Login as 'anto'. Use pageant
to save time logging in with password.

Try to list the home folder contents:

```
ls
```

Try to list the shared folder contents:

```
 ls /mnt/Antonio/
 ```

Try to list the writable shared folder contents:

```
 ls /mnt/wAntonio/
 ```
If it gets stuck then you need to remount the shared folder that
is stuck. Close and reopen terminal.

After this you can check
if the share works by unmounting an mounting the folders.
Gain root access typing `su` and putting the root password.

As root unmount broken folder:

```
umount /mnt/Antonio
```

and the same for `wAntonio`. When unmount fails because of device busy we
can force the unmount with:

```
umount -fl /mnt/Antonio
```

To remount the folder:

```
mount -a
```

Use `ls` to check that remounting was successful.

If there is an error, we can try to debug it by mounting the share folder
manually. As of 2017-11-29, the correct command is:

```
mount -t cifs //laser2002j-pc.ad.chem.ucla.edu/Antonio /mnt/Antonio/ -o user=temp,password=temp123,domain=laser2002j-pc,vers=2.1
```

We had to add the `vers=2.1` due to a recent windows update. Then we added the
`domain` option which was not needed before.


References:
- http://www.tldp.org/HOWTO/SMB-HOWTO-8.html

It may be a protocol version issue:
https://serverfault.com/questions/414074/mount-cifs-host-is-down

## JupyterHub

When Jupyter gives a server error (i.e. want load notebook list, or open
notebooks), most of the times is because the shared folders are not properly
working, so jupyterhub gets stuck trying to display the file list.
The same issue happens if you try `ls` in the `/home/anto`.

In this case refer to the previous section to re-mount the shared folders.

If after solving the shared folder issue, you want to restart jupyterhub
follow the steps below.

- Connect to geppetto with putty

- List screen sessions:

```
screen -ls
```

Usually there is only one session that is the one running jupyterhub. To open
it ("reattach" it) :

```
screen -R
```

If there are more sessions, open the one with jupyterhub in the name:

```
screen -R jupyterhub
```

**NOTE 1**: Be carefull not to call screen from inside a screen session
otherwise you end up with confusing nested `screen` sessions.
To terminate the current screen session type `exit`.
To detach a `screen` session (without terminating it) use code
CTRL+a, d (CTRL+a starts command mode, then the next character is the
command). If lost, do CTRL+a, d a few times until you are sure you are
not in a screen session anymore.

**NOTE 2**: You cannot attach an already attached `screen` session.
If this is the case check other open terminal windows with that session.
You can also force detaching an attached session with

```
screen -d sessionname
```

- Stop jupyterhub with CTRL+c (wait a few seconds)

- Start jupyterhub again:

```
jupyterhub
```
