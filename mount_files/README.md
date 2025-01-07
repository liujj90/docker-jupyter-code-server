# JJ's Jupyter + Code-Server implementation
A sandbox environment for code development on the go. Simply connect to the exposed ports (password protected) and start coding. 

# Code-server
- Base Image: codercom/code-server:latest@sha256:6b8c0e944caec80057e71d2c2f352cee38fe00ae4b7515fc4458eb300844f699
- Description: Customized for python environments and setting up user.


To begin: You need to set your path to conda environments so they may be shared between jupyter and code server. 

Execute in code-server terminal: 
```
conda init
```
The user (jj) is sudo enabled. 


# Jupyter server
- Base Image: quay.io/jupyter/minimal-notebook:latest
- Description: Jupyter interface for notebooks. Code-server does not render notebooks unless it is secured by https. While this is possible through reverse proxy and ssl certificates, I found that the added complexity slowed down the whole code-server implementation to the point it was unusable (to revisit this later). 

To begin: refer to logs for token upon startup of jupyter server. assign a password to secure
```
conda init # optional
conda create --name {env} --yes
source activate {env} # or conda activate if it has been init
pip install ipykernel
python -m ipykernel install --name {env}
```

# Shared python environment behaviour
Suggested usage: keep a list of environment creation requirements files and manage environment using jupyter server
1. Note that code-server can use environments created in jupyter but not the other way around due to the shared PATH to python
2. Code-server cannot modify the packages in jupyter in order to keep ownership of the python environment clear

## Resources
Code-server: https://hub.docker.com/r/codercom/code-server/
Jupyter server: https://jupyter-docker-stacks.readthedocs.io/en/latest/index.html#