# Tutorial: Setting up a Python Environment on RStudio Server

## _By Yipeng Song & Dan Metes_

There is a huge ecosystem of machine learning (ML) and artificial intelligence (AI) algorithms in Python for predictive modeling of administrative healthcare data sets. If you're working with administrative healthcare datasets and want to utilize these great tools developed in Python, you'll be glad to know that Python is now supported by RStudio Server. Here's a step-by-step tutorial to help you set up your Python environment on RStudio Server in Alberta Health.

## Necessary Dependencies for Python Programming on RStudio Server

The Linux server that hosts RStudio Server already have Python 3.7.6 and conda installed for package and environment management. To execute Python scripts, you will only need to install the reticulate R package on RStudio Server by running the following command in the terminal: ```install.packages('reticulate')```

## Creating a Python Virtual Environment Using `conda`

Maintaining the correct version of Python packages is crucial for data analysis in Python. To ensure your Python scripts and pipelines will always work, it's a good idea to use a virtual environment. Since conda is already installed on the server, you can use it to manage your Python virtual environment. Here's how:

```conda create --name myenv python==3.7.6 ``` will create a virtual environment named “myenv” with 3.7.6 version Python on your home folder. It is ok to use other Python versions, however you will have to manually set up the environment variable “RETICULATE_PYTHON” for the reticulate package to find the correct Python. See the following section for more details. 

```conda env list ``` will check available virtual environments on your home folder.

```conda activate myenv``` will activate your virtual environments.

```pip install XXX``` will install package XXX into your virtual environment.

Check the online document https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html to learn more about managing virtual environment using ```conda```.

## Using Your Python Virtual Environment on RStudio Server

### Interactive Mode

To set up your virtual environment for interactive mode on RStudio Server, create an R file named "config_Python_env.R" in your working directory and copy and paste the following script to it. Then run this R script:
```r

library(reticulate)

Sys.setenv(RETICULATE_PYTHON="~/.conda/envs/myenv/bin/python") #Python
use_python("~/.conda/envs/myenv/bin/python") # Which Python interpreter
use_condaenv("~/.conda/envs/myenv") # Which Python virtual environment
repl_python() # enter interactive mode for running Python script

```

By default, the reticulate package will use "/opt/python/3.7.6/bin/python" as the Python interpreter to execute Python scripts. When the Python version in your virtual environment is not 3.7.6, you should set up the correct Python interpreter by yourself. Your virtual environment will be installed in "~/.conda/envs" in your home directory.

### Batch Mode

It's recommended to run your Python scripts in batch mode on the terminal. To do this, activate your Python virtual environment, then use the Python interpreter to run the script in batch mode. Here's how:

```sh

conda activate myenv 
Python path_to_working_folder/your_python_script.py

```

## A Working Example 
Let's walk through an example of building classification ML models to predict who survived the Titanic disaster, using Python on the RStudio server. Follow the steps below to create a working directory, obtain the Titanic data, install the "dummyML" Python package, and build the ML models.

* Create a working directory named "titanic_prediction" in your home folder and change the working directory to this folder in the terminal:

    * ```mkdir titanic_prediction``` to create this folder

    * ```cd titanic_prediction``` to enter this folder 

    * ```mkdir data``` to create a data folder inside titanic_prediction

* Obtain the Titanic data for the prediction task. You can either get the R script to generate the data by running the following code in the terminal or make a direct copy of the data:

    * Get the code on the terminal ```cp /home/ysong1/test_dummyML/ 0_prepare_titanic_data.R .```, then run the R script to generate the titanic data. Please remember to set the working directory to the titanic_prediction folder before run the R script. 

    * Or directly copy the data on the terminal ```cp /home/ysong1/test_dummyML/data/titanic.csv ./data```

* Install the "dummyML" Python package, which is a Python package for automated data analysis in Python. To do this, create a virtual environment and install the package using the following commands:

    * ```sh
        conda create --name mlenv python==3.7.6
        conda activate mlenv        
        pip install dummyML
      ``` 

* Build ML models using the "dummyML" package installed in your virtual environment.
    * Get the script for testing dummyML package by ```cp /home/ysong1/test_dummyML/1_test_dummyML.py .``` on the terminal to copy the script to your project folder.

    * Activate the interactive mode using the R script in the section **Using Python virutal environment on Rstudio Server**. Change myenv to mlenv.

    * Run the script interactively and check the results.

