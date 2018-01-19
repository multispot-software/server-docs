# Users and permissions

The folder `/opt` contains folders shared among different server
 users. It also contains custom system-wide files.

- source folder `/opt/src`
- conda installations in `/opt/conda`
- system-wide scripts `/opt/bin`

## Conda installations

The system-wide conda installation is in `/opt/conda/miniconda3`.
This is writable by the user `conda-admin`. Only `conda-admin`
can install new packages here but all the user can use them.

Users, can create new enviroments and these will be stored
in their home folder automatically by conda. For example,
if the user `maya` runs:

```
conda create -n py36-test python=3.6 numpy
```

the new environment will be saved in `home/maya/.conda/envs/py36-test`.

## Source folder

The `/opt/src` folder contatins the sources for different tools.

- Each subfolders is a git repository.
- All the members of the `src` group have write access to `/opt/src`

Each user can use git to commit modifications. In this way the
git history will track the author of the commit.

Different users may have different commit privileges to github.
Most repository are part of the `multispot-software` organization
on github, so each user on the server should be anbled with
commit provileges on github.

## Bin folder

This folder contains executables that should be available to
all users. For example, scripts in `/opt/src/transfer_convert`
should have a symbolc link in `/opt/bin` for ease of use.
This will avoid the need to specify the absolute path when
calling a command, i.e. it will not be necessary to enter the
source folder and preprend the command with `./`.

Each user that wants to take advantage of this, needs to add
`/opt/bin` to their PATH in `~/.bashrc`. These commands may still
require the activation of a conda environment.

Since files in `/opt/bin` are links, their permissions are the
permission of the target file. Users that can execute
the original files, can also execute the files in `/opt/bin`.
