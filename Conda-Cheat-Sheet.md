<- [Home](home)

Another cheat sheet for conda can be found [here](https://conda.io/projects/conda/en/latest/user-guide/cheatsheet.html). And the full conda documentation [here](https://docs.conda.io/projects/conda/en/latest/index.html).

## Most used commands
General conda commands:

    conda env list # list all environments

Managing environments:
    
    # Create a new environment
    conda create --name <env_name>
    conda env create -f environment.yml # From a file
    conda create --name <env_name> --file environment.yml # From a file with a specific name 
    conda create --name <env_name> python=3.11 # With a specific python version
    # Delete an environment
    conda env remove --name <env_name> # remove an environment

Activate an environment:

    conda activate <env_name> # activate an environment
    conda deactivate # deactivate an environment

> [!WARNING]  
> On the IEA Laptops, Windows Powershell cannot activate conda environments. Use the basic Command Prompt instead. When using any IDE, make sure that the included terminal is set to use the Command Prompt.

## Export environments

    conda env export > environment.yml # export an environment to a file
    conda env update --file environment.yml # update an environment from a file

## Update conda

There was sometimes the problem that Conda in the Software Center was a very old version (from 07/2020 or 4.8.3). It was possible to update this yourself, but it could lead to many dependency conflicts that Conda itself could not solve. Before you try this (described below), first write to IT to see if they can provide a new version for you. For some reason this happens on a per user basis.

If you need to update conda yourself (better to solve that with IT), specify the version number of conda to update to. Checkout the latest version number in the [conda changelog](https://docs.conda.io/projects/conda/en/latest/release-notes.html) and then run the steps below. If errors occur, first run the commands in the troubleshooting section below. If this still not works, check your current version with `conda -V` and try to update step by step with version numbers which are not too far apart. On a yearly basis should be fine. Do this until you reach the latest version.

    conda install -n base conda==<version_number>
    conda update -n base --all 

When you reached the latest version, the next updates form here should be fine. This is just an issue with the very old version from the Software Center.

### Update environment packages

    conda activate <env_name>
    conda update --all 

## Troubleshooting

### Add conda-forge channel

It is recommended to add the conda-forge channel to the conda configuration. This is a community led channel with many packages which are not available in the default channels. This can be done with:

    conda config --add channels conda-forge

### Clean up conda
Specially during the first conda update or general, if weird unexpected errors occur, you might want to

.. reset all conda files:

    conda init --reverse

.. delete the cache of downloaded packages:

    conda clean --all

### Use faster conda resolver (mamba)

The default conda resolver is very slow. To use the faster mamba resolver, install it first:

    conda install -n base conda-libmamba-solver

Which you can now use manually during a conda installation:

    conda install <package_name> --solver=libmamba

Or you can set it as the default solver:

    conda config --set solver libmamba

In general mamba is a much better alternative to conda. But right now it is not recommended to use it on top of the default conda installation. And it is not available in the Software Center at the moment.
