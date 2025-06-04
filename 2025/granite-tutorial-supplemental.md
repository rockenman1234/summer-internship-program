# Granite Tutorial Supplement (BETA)

This page covers some added resources to run and work with the open source IBM Granite Workshop material [see workshop material here](https://ibm.github.io/granite-workshop).

## Running on PACE ICE

Students with the VSIP program have access to the ICE cluster, which has CPU and GPU backends as well as an ollama instance supported through Open OnDemand. This Ollama instance can be used in place of running locally on your laptop or Replicate, which uses a GPU backend.

[Using Ollama + Jupyter in Open OnDemand](https://gatech.service-now.com/home?id=kb_article_view&sysparm_article=KB0044442) (requires GT login)

> [!NOTE] 
> PACE supports ollama endpoints via Slurm scheduled jobs rather than as a locally (laptop) or remotely hosted (Replicate) service. You can use the Open OnDemand (OOD) application on PACE ICE to schedule a Slurm job that then has access to a set of shared models, including the latest Granite 3 models.

### Setting up your PACE virtual environment

The workshop highly suggests that you create a "clean" venv or virtual environment to run the workshop notebooks. Once you log in to PACE ICE, you can run the following from a terminal on the login node:

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
The easiest way to use Ollama with Jupyter notebooks is to use the [Open OnDemand instance for ICE](https://gatech.service-now.com/home?id=kb_article_view&sysparm_article=KB0042133) (Requires GT login for docs and OOD interface). Once you log in to the OnDemand instance, you can select the following: `Interactive Applications->Ollama + Jupyter (Beta)`

---
We recommend running with the following parameters:
- Python environment: Custom virtual environment
    - Virtual environment path: `/home/hice1/<gburdell>/granite_venv`
- Ollama models directory: `PACE Shared Models`
- Jupyter interface: `Jupyter Notebook`
- Node type: <Choose 1 of "NVIDIA L40s", "NVIDIA GPU H100 (HGX)", or "NVIDIA GPU (first avail)">
- Number of CPUs (cores): `4`
- Number of GPUs: `1`
- Total memory: <32 GB or 64 GB>
- Time: <start with 1 hour and expand as needed>
- Quality of service: `Default (none)`
---

> [!TIP] 
> ICE uses the OLLAMA_MODELS environment variable to point to a shared storage location for models

### Changes to notebooks on ICE

The Granite workshop materials already check for a "local" ollama instance or for Replicate, and the existing tutorials use common variables like OLLAMA_HOST and OLLAMA_MODELS to point to the local Ollama instance and models directory. 

The main change we need to make for our notebook is to point to the correct shared models directory.

From the [Summarize example notebook](https://ibm.github.io/granite-workshop/lab-1/):
```
import os
import requests
from langchain_ollama.llms import OllamaLLM
from langchain_community.llms import Replicate
from ibm_granite_community.notebook_utils import get_env_var

#model_path = "ibm-granite/granite-3.3-8b-instruct"
## Replace model path with pointer to shared model
model_path = <TBD>
try: # Look for a locally accessible Ollama server for the model
    response = requests.get(os.getenv("OLLAMA_HOST", "http://127.0.0.1:11434"))
    model = OllamaLLM(
        model="granite3.3:8b",
        num_ctx=65536, # 64K context window
    )
    model = model.bind(raw=True) # Client side controls prompt
```

> [!TIP] To print out all the available models for Ollama you can run the following:
```
import ollama
for m in ollama.list().models:
    print(m.model)
```

## Running locally

The Granite workshop recommends running on a laptop with at least 32 GB of memory and possibly a GPU. Note that some of the workshop examples may take 10-20 minutes to run on a laptop with less memory or no integrated GPU.

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
