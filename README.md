# CFC

IMPORTANT: 

correct for the tiny diff between scale rel and pressure profile normalization (see above and noteboook class_sz_tszpowerspectrum_plancklevel_and_benchmark.ipynb (laptop))
A_ym_sims_over_actual = 1.0181359966586783
correct for the bias that was set only in the amplitude but not in the angle/radius
A_ym_sims_over_actual *= (1./1.25)**(-(1.+0.12))

## environment file

You should create an environment file that sets up the corretc environment variables and activates the virtual environment. 

In .bash_socfc:

```bash
source /home/bb667/rds/bb667/cluster_counts/SOCFC/SOCFC_env/bin/activate
export PATH_TO_CLASS_SZ_DATA=$GOTOSOCFC/class_sz/class_sz_data_directory
export PYTHONPATH=$GOTOSOCFC/class_sz/class-sz/python/classy_szfast:$PYTHONPATH
```

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


## requirements

We have a requirements.txt 

pip install -r requirements.txt


## Nemo 

git clone https://github.com/SOCFC/nemo.git
cd nemo 
pip install -e .


## nemo-sim-kit

This doesnt need to be installed. 
It is just a collection of scripts that are used to generate the simulations. 

git clone https://github.com/SOCFC/nemo-sim-kit.git


## class_sz 

Follow these instructions carefully. 

```bash
git clone https://github.com/CLASS-SZ/class_sz
git clone https://github.com/CLASS-SZ/get_cosmopower_emus.git

cd get_cosmopower_emus
pip install -e .
cd ..

git clone https://github.com/CLASS-SZ/class_sz_data.git
cd class_sz_data
pip install -e .
cd ..

cd class_sz/class-sz/python
git clone https://github.com/CLASS-SZ/classy_szfast
cd ..

chmod +x select_makefile.sh
./select_makefile.sh

chmod +x download_emulators.sh
source download_emulators.sh

export PATH_TO_CLASS_SZ_DATA=$PWD/../class_sz_data_directory

make clean 
make -j 

cd python/classy_szfast
pip install -e .
cd ../..

export PYTHONPATH=$(pwd)/python/classy_szfast:$PYTHONPATH
```

You should then be able to run: 


```bash
$ python
>>> import classy_sz
```

Now, for this installation to work next time. You need to save the following environment variables and source the environment file. 

First retrieve the environment variables: 

```bash
echo $PATH_TO_CLASS_SZ_DATA
```
It should print something like:

```bash
/path/to/class_sz/class_sz_data_directory
```

also the PYTHONPATH:

```bash
echo $PYTHONPATH
```

It should print something like:

```bash
/path/to/class_sz/class-sz/python/classy_szfast:
```

Save the printed path and put the following the environment file (e.g., create a file called .class_sz_env.sh and paste the following in it):

```bash
export PATH_TO_CLASS_SZ_DATA=/path/to/class_sz/class_sz_data_directory
export PYTHONPATH=/path/to/class_sz/class-sz/python/classy_szfast:
```

Of course, you need to replace `/path/to/..` with the actual path. 

Then, next time you want to work with class_sz, just source the environment file before running python:

```bash
source .class_sz_env.sh
```

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

## Running calculations

First install jupyterlab

```bash
pip install jupyterlab
```

Then start jupyterlab (after activating the SOCFC environment)

```bash
source /path/to/your/.bash_socfc # or your environment file
jupyter-lab
```

You can then open the SOCFC_env kernel and run calculations. 
For instance, you can open the class_sz/docs/notebooks/classy_szfast_hz.ipynb notebook and run the calculations. 

