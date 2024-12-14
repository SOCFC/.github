# CFC

**Important to keep in mind before painting:** 

In first run boris has overlooked the following. Make sure we dont do the same. 
```bash
correct for the tiny diff between scale rel and pressure profile normalization (see above and noteboook class_sz_tszpowerspectrum_plancklevel_and_benchmark.ipynb (laptop))
A_ym_sims_over_actual = 1.0181359966586783
correct for the bias that was set only in the amplitude but not in the angle/radius
A_ym_sims_over_actual *= (1./1.25)**(-(1.+0.12))
```
## environment file

You should create an environment file that sets up the corretc environment variables and activates the virtual environment. 

In .bash_socfc:

```bash
source /path/to/SOCFC/SOCFC_env/bin/activate
cd $GOTOSOCFC
```

where `$GOTOSOCFC` is the path to the SOCFC directory. 


Then to start the SOCFC environment:

```bash
source /path/to/your/.bash_socfc
```


## CFC environment 

We are in:

/home/bb667/rds/bb667/cluster_counts/SOCFC

(mkdir SOCFC if it is not there already.)

Python version 3.11


Create virtual environmnent:

cd SOCFC
python3.11 -m venv SOCFC_env

source /home/bb667/rds/bb667/cluster_counts/SOCFC/SOCFC_env/bin/activate


## scripts 

To download the pipeline scripts, do:

```bash
git clone https://github.com/SOCFC/sz-cfc.git
```


## requirements

We have a requirements.txt in our scripts.

pip install -r sz-cfc/requirements.txt


## Nemo 

git clone https://github.com/SOCFC/nemo.git
cd nemo 
pip install -e .


## nemo-sim-kit

This doesnt need to be installed. 
It is just a collection of scripts that are used to generate the simulations. 

git clone https://github.com/SOCFC/nemo-sim-kit.git




## Jupyter kernel

We need to set up a jupyter kernel for SOCFC. 

First activate the SOCFC virtual environment. Then do:

```bash
pip install ipykernel
python -m ipykernel install --user --name SOCFC_env --display-name "Python (SOCFC)"
```

This should print something like:

```bash
Installed kernelspec jupyter-SOCFC_env in /home/bb667/.local/share/jupyter/kernels/jupyter-SOCFC_env
```

You can navigate to the .local/share/jupyter/kernels/jupyter-SOCFC_env directory and open the kernel.json file.
You can add the OMP_NUM_THREADS environment variable to the kernel.json file as here: 

```json
{
 "argv": [
  "/home/bb667/rds/bb667/cluster_counts/SOCFC/SOCFC_env/bin/python",
  "-Xfrozen_modules=off",
  "-m",
  "ipykernel_launcher",
  "-f",
  "{connection_file}"
 ],
 "display_name": "Python (SOCFC)",
 "language": "python",
 "metadata": {
  "debugger": true
 },
 "env": {
  "OMP_NUM_THREADS": "32"
 }
}
```


## class_sz 

Class_sz is pip installed from the requirements.txt

Nevertheless, before you import class_sz in python, you need to set the PATH_TO_CLASS_SZ_DATA so that all the package data is downloaded and linked. 

We store the data in our root directory `SOCFC`:

In a terminal, run:

```bash
export PATH_TO_CLASS_SZ_DATA=/path/to/SOCFC
mkdir -p $PATH_TO_CLASS_SZ_DATA/class_sz_data_directory
```

then: 

```bash
$ python
>>> import classy_sz
```

Now, for this installation to work next time, we should export the PATH_TO_CLASS_SZ_DATA in our socfc environment file. 

First retrieve the environment variables: 

```bash
echo $PATH_TO_CLASS_SZ_DATA
```
It should print something like:

```bash
/path/to/class_sz/class_sz_data_directory
```

```

Save the printed path and put the following the environment file (e.g., create a file called .bash_socfc and paste the following in it):

```bash
export PATH_TO_CLASS_SZ_DATA=$GOTOSOCFC/class_sz_data_directory
```

Of course, you need to replace `/path/to/..` with the actual path. 

For instance, a .bash_socfc file could be:

```bash
source /home/bb667/rds/bb667/cluster_counts/SOCFC/SOCFC_env/bin/activate
export PATH_TO_CLASS_SZ_DATA=/home/bb667/rds/bb667/cluster_counts/SOCFC/class_sz_data_directory
cd $GOTOSOCFC
``` 


## Running calculations

Activate the SOCFC environement (e.g., source .bash_socfc). Launch jupyterlab. 

Then navigate to the sz-cfc directory and open the notebook class_sz.ipynb to try a class_sz calculation. 
