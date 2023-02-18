# Upload a Package to PyPi

This repository is a guide to build a Python package and to upload the package to PyPi.


There are further helpful links here:
- [package building summary guide](https://packaging.python.org/guides/distributing-packages-using-setuptools/) from the Python website

There is also a detailed explanation of distributing Python packages from Distutils:

- [Introduction](https://docs.python.org/3/distutils/introduction.html)
- [setup.py script](https://docs.python.org/3/distutils/setupscript.html)
- [config file](https://docs.python.org/3/distutils/configfile.html)
- [source distributions](https://docs.python.org/3/distutils/sourcedist.html)
- [built distributions](https://docs.python.org/3/distutils/builtdist.html)
- [uploading to PyPi](https://docs.python.org/3/distutils/packageindex.html)


## Module vs. Package

In Python, a module is a single Python file that contains a collection of classes, functions and/or global variables. They are called modules because they are modular. That means one can reuse these files in different applications.

A package is a collection of modules placed into a directory.

## Create a Module

In this repo package example there are the files in the same ```distributions``` directory:
- Gaussiandistribution.py
- Generaldistribution.py

The Gaussiandistribution.py file imports the Distribution class of Generaldistribution.py via
```python
from .Generaldistribution import Distribution
```

Generaldistribution.py is a module for Gaussiandistribution.py

## Making a package

1. In order to create a Python package an ```__init__.py``` file is needed in the ```distributions```folder.

    In this example its content is:
    ```python
    from .Gaussiandistribution import Gaussian
    from .Binomialdistribution import Binomial
    ```
    - The reason for adding these lines is to import the Gaussian/Binomial classes directly.
    - Without these lines the program will still work.
    - But one would have to import (e.g.) the Gaussian class more indirectly with the line ```from distributions.Gaussiandistribution import Gaussian```.
    - It's like a shortcut for imports.


2. One level back in the directory there is a ```setup.py```file.
    ```python
    from setuptools import setup

    setup(name='distributions',
      version='0.1',
      description='Gaussian distributions',
      packages=['distributions'],
      zip_safe=False)
    ```
    - It's necessary for pip installing.
    - Pip will automatically look for this file.
    - It contains information or metadata about the package like the package name, version, description, etc.
    - Every package on PyPi needs a unique name. So change the name if needed.
    - Use here the same name as the package name.
    - zip_safe=False means - it cannot be run directly from a zip file
    - Further details can be found here: [tutorial on distributing packages](https://packaging.python.org/tutorials/packaging-projects/)


3. Create a virtual environment

    Using Anaconda
    ```sh
    conda create --name environmentname
    conda activate environmentname
    ```

    Using Pip and Venv
    - cd to the directory with the ```setup.py``` file
    - In the terminal enter
    ```sh
    pip install virtualenv
    virtualenv environmentname
    source environmentname/Scripts/activate
    ```


4. Install the package locally (first test)

    - cd to the directory with the ```setup.py``` file
    - In the terminal enter
    ```sh
    pip install .
    ```
    The dot tells pip to look for the setup file in the current folder.


5. Test the package installation like so

    - Open a new terminal (it does not matter to be in the package example folder, as the package is installed)
    - Type in ```python``` to start the interpreter
    - Try for example
    ```python
    from distributions import Gaussian
    ```
    - If no error occurs the package is successfully installed


6. To find the location of the package installation try
    ```python
    import distributions
    distributions.__file__
    ```

## Making changes in the package and test them locally
Every time you change something in the modules ```Gaussiandistribution.py```or ```Generaldistribution.py``` do the following to activate the changes.
  - cd to the directory with the ```setup.py``` file
  - In the terminal enter
  ```sh
  pip install .
  ```

## Use local pip installations in other conda enviroments
If you want to use your local packages in other conda environments you should do the following:
  - Activate you conda environment
  ```sh
  conda activate YOUR_ENV_NAME
  ```
  - cd to the directory with the ```setup.py``` file
  - In the terminal enter
  ```sh
  pip install .
  ```

## Upload to PyPi

7. Register on PyPi vs. Test PyPi
    Go to the websites and register there
    - [pypi.org](https://pypi.org/)
    - [test.pypy.org](https://test.pypi.org/)


8. In the distributions folder add a ```license.txt``` file.
    - Add a license text
    - Use the [MIT license](https://opensource.org/licenses/MIT)


9. In the distributions folder add a README.md file
    - This file contains the description and usage of the package
    - It will be shown on PyPi


10. Create a file called ```setup.cfg```

    It contains data about the README.md file
    ```
    [metadata]
    description-file = README.md
    ```

11. ```cd``` to the folder with ```setup.py```. Write in the terminal:
    ```sh
    python setup.py sdist
    pip install twine

    # commands to upload to the pypi test repository
    twine upload --repository-url https://test.pypi.org/legacy/ dist/*
    pip install --index-url https://test.pypi.org/simple/ distributions

    # command to upload to the pypi repository
    twine upload dist/*
    pip install distributions
    ```

    - You need username and password
    - Both for PyPi and Pypi test


12. In case of an error (e.g. a dublicate):
    - Change e.g. version number
    - Delete ```dist```folder
    - Delete ```...egg-info``` folder
    - Repeat step 11


13. Login to PyPi (or Test PyPi) and check the upload


14. For a package installation write the following
    ```sh
    pip install distributions
    ```

## Setup Instructions to get a local repo

The following is a brief set of instructions on setting up a managed notebook instance, from which the notebooks can be completed and run.

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites: Installation of Python via Anaconda and Command Line Interaface
- Install [Anaconda](https://www.anaconda.com/distribution/). Install Python 3.7 - 64 Bit
- If you need a good Command Line Interface (CLI) under Windowsa you could use [git](https://git-scm.com/). Under Mac OS use the pre-installed Terminal.

- Upgrade Anaconda via
```sh
conda upgrade conda
conda upgrade --all
```

- Optional: In case of trouble add Anaconda to your system path. Write in your CLI
```sh
export PATH="/path/to/anaconda/bin:$PATH"
```

### Clone the project
- Open your Command Line Interface
- Change Directory to your project older, e.g. `cd my_github_projects`
- Clone the Github Project inside this folder with Git Bash (Terminal) via:
```sh
git clone https://github.com/ddhartma/PyPi-Package-Upload.git
```

- Change Directory
```sh
cd PyPi-Package-Upload
```

- Create a new Python environment. Inside Git Bash (Terminal) write:
```sh
conda create --name pypi_package_maker
```

- If you want to execute the Distribution example then install the following packages (via pip or conda)
```
numpy = 1.17.4
pandas = 0.24.2
matplotlib = 3.1.0
tensorflow = 1.14.0
seaborn = 0.9.0
scikit-learn = 0.21.2
```

- Check the environment installation via
```sh
conda env list
```

- Activate the installed crisp_dm_analysis environment via
```sh
conda activate pypi_package_maker
```
