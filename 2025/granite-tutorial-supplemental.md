

## Running on PACE ICE

Students with the VSIP program have access to the ICE cluster, which has CPU and GPU backends as well as an ollama instance supported through Open OnDemand. 

[Using Ollama + Jupyter in Open OnDemand](https://gatech.service-now.com/home?id=kb_article_view&sysparm_article=KB0044442)

We recommend running with the following parameters:
Python environment: Default
Ollama models directory: PACE Shared Models
Jupyter interface: Jupyter Notebook 
Node type: NVIDIA L40s
Number of CPUs (cores): 4
Number of GPUs: 1
Total memory: 32 GB


## Running locally

The Granite workshop recommends running on a laptop with at least 32 GB of memory. 

## Possible local errors

**Note:** most of these errors are tested with MacOS

### Python version does not match the expected Python 3.10-3.13

```
Create a Python environment with an explicit version of Python and then name it "granite_venv"
```
python3.10 -m venv --upgrade-deps --clear venv granite_venv
source granite_venv/bin/activate
```
