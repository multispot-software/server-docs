# How to add system-wide environment

New conda environments create by `conda-admin` user are available to all users. 

As a first step, we need to become `conda-admin`:

```
$ su conda-admin
```

Now we can create a new conda environment:

```
$ conda create -n my_new_env python=3.6 ipykernel
```

The previous command installs python 3.6 and ipykernel in the new
environments. `ipykernel` is needed to use the environment from the
notebook.

The kernel for the new env need to be installed in the notebook server
as follows:

```
$ conda activate my_new_env
$ python -m ipykernel install --name my_new_env --display-name "Python (my_new_env)"
```

Reloading the jupyter dashboard page should be enough to see the new environment.
Jupyterhub itself does not need to be restarted!

To remove the kernel from jupyter:

```
$ rm -rf /usr/local/share/jupyter/kernels/my_new_env
```

To remove the conda environment:

```
$ conda env remove -n my_new_env
```

Reference:
- https://ipython.readthedocs.io/en/latest/install/kernel_install.html

