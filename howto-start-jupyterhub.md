# Starting JupyterHub

Jupyterhub need to the started by root. First become root:

```
$ su
```

Then, we start a screen session:

```
# screen -S jupyterhub -L
```

Then we activate the jupyterhub environment:

```
# . /opt/conda/miniconda3/etc/profile.d/conda.sh 
# conda activate jupyterhub
```

Start jupyterhub:

```
# jupyterhub
```

Finally detach the screen session (`CTRL+a`, then `d`) end exit from
`root` with:

```
# exit
$
```

The $ sign means that you are now a normal unprivileged user.
