# Jupyter

## Use pipenv as a jupyterlab kernel

```sh
# First install jupyterlab
python3 -m pip install jupyterlab

# Install the necessary packages
# ipykernel for kernel management
# ipympl for matplotlib widget
pipenv install ipykernel ipympl

# Install the jupyter extensions (requires nodejs 5+)
jupyter labextension install @jupyter-widgets/jupyterlab-manager
jupyter labextension install jupyter-matplotlib

# Actually create the ipykernel
pipenv run python -m ipykernel install --user --name NAME --display-name "Python 2 (NAME)"

# If there is a git repository, install package to
# prevent committing notebook outputs
pipenv install nbstripout
nbstripout --install --attributes .gitattributes

# List available kernels
jupyter kernelspec list
 
# Remove a kernel
jupyter kernelspec remove NAME
```
