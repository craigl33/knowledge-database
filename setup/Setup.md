<- [Home](home)

Here is a short overview on how to set up everything and install the necessary packages. At the end is also a troubleshooting section.


## Project Folder

Every setup process starts with the project folder. It is a dedicated directory for organizing and managing all script files and other project files. Data can be put somewhere else. Although scripts can be reverenced over multiple folders and can still be run with Jupyter Notebooks or as Python files (like done with the old R scripts), this has some drawbacks. Or rather you give up some advantages:

- Simplicity and better organization, the complete code is only in one place. No confusion
- No potential problems with interfering with other projects
- It is ideal for using version control (which alone is extremely helpful)
- Encourages good and up-to-date documentation practices (Write code and documentation in the same project/ editor).
- ...

When creating a new project folder, you can simply copy all files from the source or even better use version control (see here: [git](Version-Control)).


## Integrated Development Environment (IDE)

Which IDE is then used does not really matter. Regardless of whether you choose PyCharm, Visual Studio Code, Spyder or even Jupyter Notebooks, they all revolve around the project folder. Currently, there is an up-to-date version of PyCharm in the Software Center of the IEA. Visual Studio Code is unfortunately a bit outdated, but still can be used

Besides the countless things most IDEs can do anyway, most support additional plugins. Here are some recommendations:

- GitHub Copilot: AI-based code completion (currently only available for VS Code and PyCharm). By far the most useful plugin. Not perfect, but useful to learn new things and to speed up the coding process. (Download: [PyCharm](https://plugins.jetbrains.com/plugin/17718-github-copilot), [VS Code](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot))


## Get the code
For getting any code the question is if python packaging with pip is used or not. Right now for all the code within rise-iea, the code is directly imported into the project folder and no python packaging is used to avoid a setup overhead. Below are some advantages and disadvantages for both approaches. But this is only relevant if this might be changed in the future.

Python Packaging:
- The code is structured as a python package and can be installed via pip.
- Pros:
  - Package dependencies are managed automatically
  - Distribution and versioning is easier
    - Better integration with any tools (Linters, test runners, CI/CD, ...)
    - For production the script can be used like any other python package (numpy, pandas, ...)
- Cons:
  - More complex setup process
  - Environment pollution if not used correctly
  - Can be confusing

Direct import:
- The code is just imported directly into the project folder.
- Pros:
  - Simpler setup process
  - No installation required
- Cons:
  - Needs manually installation of dependencies
  - Missing all the advantages of python packaging

#todo Add documentation here, once its more clear on how the general code structure will look like and if it is shared a lot/ used in production.


## Virtual Environment

A virtual environment is a self-contained, isolated environment where you can install Python packages and manage dependencies separately from your system-wide Python installation. If you don't use virtual environments all installed packages (pandas, julia, jupyter notebook). Having an own installation of those packages for each project has some advantages again:

1. Dependency Isolation: Virtual environments allow you to install specific packages and their versions for a particular project without affecting other projects or the system-wide Python installation. Otherwise, it can sometimes happen that through a package update something in another project no longer works. This can cause bugs that are sometimes very unpleasant to solve.
2. Easy setup: Virtual environments are portable, meaning you can easily transfer them to other machines, ensuring consistent setups across different environments.
3. Version Compatibility: They enable you to create an environment with a specific Python version, ensuring compatibility with your project's requirements.
4. Clean Development: With virtual environments, you can experiment with different libraries and configurations without cluttering your main Python installation.

A common package and environment manager is Conda and can be installed via the IEA Software Center. Below is a short introduction into the most important commands. Find more details here: [Conda Documentation: Managing environments](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html).

If a conda environment is set up you activate it with `conda activate <env_name>` and deactivate it with `conda deactivate`. Then you can run all python commands or launch a Jupyter Notebook from there. Most IDEs also support conda environments and can be setup to use them.


### Install dependencies
In the documentation/ README file of each repository should be a list of all dependencies and documented how to install them.

In general there are some IEA specific things to to be aware off:

(1) The IEA has a proxy for all internet traffic. This means that all packages have to be installed via the proxy. This is done by passing the proxy as an argument to the install command. 
For installation via pip a proxy argument can be passed like this:

      python -m pip install --proxy http://proxy.iea.org:8080 <package>

For installation via conda the proxy can be set in the conda config:

      conda config --set proxy_servers.http http://proxy.iea.org:8080
      conda config --set proxy_servers.https http://proxy.iea.org:8080

For julia packages open the julia console and run:

      ENV["HTTP_PROXY"] = "http://proxy.iea.org:8080"
      ENV["HTTPS_PROXY"] = "http://proxy.iea.org:8080"

### Update environment.yml
Most (if not all) repositories contain a file called `environment.yml`. This file can be used to create a new conda environment with all dependencies. For the fresh installation this is by far the easiest way to install all dependencies:

      conda env create -f environment.yml # Name of created env is defined in file (same as project folder name)
      conda create --name <env_name> --file environment.yml # With a specific name 

If the environment already exists you have to delete it first or choose another name.

Not just the dependencies are installed, but also with the correct versions. This is important, because otherwise it can happen that a package update breaks something. But that also means that the `environment.yml` file should be updated regularly. Also, if a new package is added, it should be added th the file. 

To create a new `environment.yml` file, activate your environment and run:

      conda env export > environment.yml

Since the file is also included in git, you see all changes in the file and which packages have been added, updated or removed.

Of course before that, the actual dependencies have to be updated and it is also recommended to run the code to see if everything still works or adjust it if necessary. When updating to a new Python version it is the easiest to create a new environment and install all dependencies from scratch See more in the [Conda Cheat Sheet](Conda-Cheat-Sheet) section.

