

## Running on PACE ICE

Students with the VSIP program have access to the ICE cluster, which has CPU and GPU backends as well as an ollama instance supported through Open OnDemand. 

[Using Ollama + Jupyter in Open OnDemand](https://gatech.service-now.com/home?id=kb_article_view&sysparm_article=KB0044442) (requires GT login)

### Setting up your PACE venv

From a terminal on the login node, you can run the following:

```
[login-ice ~]$ module load python/3.12
# Note we've named our venv "granite_venv" here but you can name it whatever you like!
$ python -m venv --upgrade-deps --clear venv granite_env
# Source the environment to use it on your terminal
$ source ~/granite_env/bin/activate
# Make sure this venv has jupyter installed to use with the PACE OOD environment
(granite_env) [login-ice ~]$ python -m pip install --require-virtualenv notebook jupyter ipywidgets
```

### Starting an Open OnDemand instance

We recommend running with the following parameters:
- Python environment: Default
- Ollama models directory: PACE Shared Models
- Jupyter interface: Jupyter Notebook 
- Node type: NVIDIA L40s
- Number of CPUs (cores): 4
- Number of GPUs: 1
- Total memory: 32 GB

## Running locally

The Granite workshop recommends running on a laptop with at least 32 GB of memory and possibly a GPU. 

## Possible local errors

**Note:** most of these errors were experienced on MacOS.

### Python version does not match the expected Python 3.10-3.13

If you get an error when running the Granite notebooks about the Python version, you can try the following:

1) Create a Python environment with an explicit version of Python. Here we name it "granite_venv"
```
python3.10 -m venv --upgrade-deps --clear venv granite_venv
source granite_venv/bin/activate
python -m pip install --require-virtualenv notebook ipywidgets
#Then start jupyter from this venv to use this version of Python
```

2) Make sure you don't have other copies of Jupyter notebook or lab installed that use other versions of Python

```
brew list | grep jupyter
brew remove jupyter
```


