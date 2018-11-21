## HPC Software



## System Packages

## C / C++ / C#

#### BLAST

##### Blast Example

## Python

### Conda

This is one of the best package managers i have used. It has many uses in an HPC environment.

#### Conda list environments

```bash
conda env list
```

#### Conda - create new environment with python 3.7

```bash
conda create -y -n python3.7 python=3.7
```

#### Conda - create new environment with python 3.7 and install jupyter.

```bash
conda create -y -n jupyter python=3.7 jupyter
```

#### Conda - Delete environment

```bash
conda env remove -y -n python3.7
```

#### Conda - Enable conda environment

```bash
source activate <name-of-environment>
```

#### Conda - Disable conda environment

```bash
source deactivate
```

### Jupyter Notebooks

The [Jupyter Notebook](http://jupyter.org/) is an open-source web application that allows you to create and share documents that contain live code, equations, visualizations and narrative text. Uses include: data cleaning and transformation, numerical simulation, statistical modeling, data visualization, machine learning, and much more. 

This can be installed on your local laptop or workstation.

These can also be used on many HPC Clusters which is exciting because there is often more software there and access to many more cpu and memory.

#### Installing Jupyter Notebooks

If Anaconda is installed then Jupyter is all ready installed.

If just conda is installed than run the following to install Juypter Notebooks.

```bash
conda create -y -n jupyter python=3.7 jupyter
source activate jupyter
```

### DASK

For the python ecosystem, consider using [Dask](http://dask.pydata.org/en/latest/) which provides advanced parallelism for analytics. [Why use Dask](http://dask.pydata.org/en/latest/why.html) versus (or along with) other options? Dask integrates with Numpy, Pandas, and Scikit-Learn, and it also:

* [scales up to clusters](http://dask.pydata.org/en/latest/why.html#scales-out-to-clusters) with multiple nodes
* [deployable on job queuing systems](https://dask-jobqueue.readthedocs.io/en/latest/) like PBS, Slurm, MOAB, SGE, and LSF
* also [scales down to parallel usage of a single-node such as a server or laptop](http://dask.pydata.org/en/latest/why.html#scales-down-to-single-computers)  modern laptops often have a multi-core CPU, 16-32GB of RAM, and flash-based hard drives that can stream through data several times faster than HDDs or SSDs of even a year or two ago.
* supports a [map-shuffle-reduce pattern](http://dask.pydata.org/en/latest/why.html#supports-complex-applications) popularized by Hadoop and is a smaller, lightweight alternative to Spark.
* works with [MPI via mpi4py library](http://dask.pydata.org/en/latest/setup/hpc.html#using-mpi) and compatible with infiniband or other high speed networks.
See example [Dask Jobqueue for PBS cluster](https://dask-jobqueue.readthedocs.io/en/latest/#example)

```python
from dask_jobqueue import PBSCluster
cluster = PBSCluster()
cluster.scale(10)         # Ask for ten workers

from dask.distributed import Client
client = Client(cluster)  # Connect this local process to remote workers

# wait for jobs to arrive, depending on the queue, this may take some time

import dask.array as da
x = ...                   # Dask commands now use these distributed resources
```

#### Dask on HPC Presentation

<iframe width="560" height="315" src="https://www.youtube.com/embed/FXsgmwpRExM" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
