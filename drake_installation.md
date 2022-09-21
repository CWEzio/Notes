# Pydrake Installation
It is best practice to install pydrake in a python virtual environment. 
## Use Python Virtual Environment
- Install virtualenv with 
  ```
  pip3 install virtualenv
  ```
- Create a virtualenv with
  ```
  python3 -m venv myenv
  ```
  This command will create a virtual environment called `myenv` in current directory.
- Activate the virtualenv with
  ```
  source path_to_myenv/myenv/bin/activate
  ```
- Deactivate the virtualenv with
  ```
  deactivate
  ```
- To delete the virtual environment, just remove the folder containing the virtual environment.

## Add python virtualenv to Jupyter notebook 
- Make sure the virtualenv is activated
- Install ipykernel with
  ```
  pip install ipykernel
  ```
- Add virtualenv to Jupyter notebook as a kernel with
  ```
  python -m ipykernel install --name=myenv --user
  ```
  where `myenv` is the name of the added kernel. 

## Install pydrake with `pip`
> Drake does not support Anaconda. <br>
> Check the [official installation guide](https://drake.mit.edu/pip.html#stable-releases).<br>

Pydrake can be installed with `pip`:
```
python3 -m venv env
env/bin/pip install --upgrade pip
env/bin/pip install drake
```
Above commands create a virtual environment called `env` and install `pydrake` in this virtual environment.

You also need some runtime libraries. Install these addtional libraries if you do not have them in your system yet:
```
sudo apt-get install --no-install-recommends \
  libpython3.8 libx11-6 libsm6 libxt6 libglib2.0-0
```
