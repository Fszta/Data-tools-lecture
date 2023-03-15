## Introduction

Pip, Anaconda, and Poetry are all package managers for Python, but they have different features and use cases.

## Pip
Pip is the default package manager for Python and is used to install and manage Python packages from the Python Package Index (PyPI). Pip is included with Python, and you can use it to install packages globally or in a virtual environment. Pip is simple to use and is suitable for most Python projects.

## Anaconda
Anaconda is a distribution of Python that includes many scientific computing packages and tools, such as NumPy, SciPy, and Jupyter notebooks. Anaconda comes with its package manager called conda, which is similar to pip but also includes features for managing non-Python packages and managing virtual environments. Anaconda is particularly useful for data science and machine learning projects because it includes many packages commonly used in those fields.

## Poetry
Poetry is a newer package manager for Python that aims to simplify dependency management and package distribution. Poetry includes features for managing virtual environments, specifying dependencies and versions, and packaging your project as a distributable package. Poetry is designed to work well with modern Python projects that use tools like Pytest and Black.


In summary, if you're working on a simple Python project, pip should suffice. If you're working on a data science or machine learning project, Anaconda might be a better choice. If you're working on a modern Python project that requires advanced dependency management and package distribution, Poetry is worth considering. Ultimately, the choice of package manager depends on your project's requirements and your personal preference.

## Commands recap

### Virtual environment creation
> You must use virtual env to manage your python projects, basically it will allow you : 
> * to prevent version conflicts
> * to create reproducible and easy to install project

There are many way to create virtual env depending on your setup, one you can use : `python3 -m venv env_name`

### Pip usage
* Activate a virtual environment : `conda activate env_name`
* Install a package : `pip install package_name`
* Install packages from requirements file : `pip install -r requirements.txt`


### Conda usage
* Activate a virtual environment : `conda activate env_name`
* Install a package : `conda install package_name`
* Install packages from requirements file : `conda install --file requirements.txt`


