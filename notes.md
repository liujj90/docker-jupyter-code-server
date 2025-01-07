## overall architecture
- jupyterlab server for notebooks -- 8888
- code-server for general development -- 8080
## Shared python environment
- Add PATH
- Initialize on same conda 
## Shared volume
- Files to be saved onto mounted volumne `/mnt/crucible/code/{{project}}`

# JUPYTER

## ENV Variables jupyter 
Used in build:
- NB_USER: jj # user for notebook
- CONDA_DIR: /opt/conda # conda directory
- NB_UID: 1000 # uid of nb user
- HOME: /home/jj/ # working directory

Used in container:
- JUPYTER_PORT: 8888
- NB_GID: 100
- PATH: /opt/conda/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
- JUPYTER_ALLOW_INSECURE_WRITES: 1

OPTIONAL:
- SHELL /bin/bash
- DEBIAN_FRONTEND
- LANG
- LANGUAGE
- LC_ALL

## WORKING DIRECTORY
/home/jj/workspace

## USER
root

## VOLUME
/mnt/crucible/docker/jupyter-code-server/{PROJECT}:/home/jj/workspace

# CODE SERVER
## ENV Variables Code-server
Required:
- TZ: Asia/Singapore
- USER: jj

Optional: 
- CONDA_AUTO_ACTIVATE_BASE
- ENTRYPOINTD
- LANG
- PASSWORD
- PATH

## USER
jj

## WORKING DIRECTORY


## VOLUME
/mnt/crucible/docker/jupyter-code-server/{PROJECT}:/home/jj/workspace