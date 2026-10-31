
Anaconda is a distribution of Python (as well as R) that is specifically suited for scientific purposes (e.g. data science, machine learning, large-scale data processing, predictive analytics) with the goal of simplifying package management and deployment

Anaconda (and Miniconda) uses the `conda` package manager, allowing us to:
- manage dependencies
- create isolated environments (ie standalone directories that contain specific versions of Python and other packages)
    - `conda create`

### Conda environment
Conda environments allow us to maintain isolated containers of specific versions of Python and 3rd party dependencies.

When you run a `conda install` command, it installs the specified package and their dependencies into the currently active conda environment, not just the current directory tree.
- we can also use `pip install` if using `pip` within a conda environment

## Resources
- [Anaconda vs Miniconda](https://docs.anaconda.com/free/anaconda/getting-started/distro-or-miniconda/)