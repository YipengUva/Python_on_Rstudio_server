# Tutorial on setting up the Python environment on Rstudio server

## _Yipeng Song & Dan Metes_

There is a huge ecosystem of machine learning (ML) and artificial intelligence (AI) algorithms in Python for predictive modeling of administrative healthcare data sets. And Python programming is now supported by the Rstudio server. Therefore, we tried to explore the possibility of using Python on the Rstudio server to make full usage of the many great ML and AI tools developed in Python. This tutorial is the result of the exploration.

## Necessary dependences on Rstudio server for Python programming

The Linux server hosted the Rstudio server has already installed Python 3.7.6 and ```conda``` for package and environment management. Therefore, the only thing needed is to install the reticulate R package on Rstudio server for executing Python scripts. ```install.packages('reticulate')```

## Create a Python virtual environment using ```conda```

Maintaining the correct version of Python packages is crucial for data analysis in Python. Your Python scripts for doing data analysis may not even work after upgrading your Python packages or installing new Python packages. Therefore, it is always a good idea to use a virtual environment for doing data analysis or developing new pipelines for other people to use. By using the same virtual environment, your script and pipeline will always work.

As ```conda``` has already been installed on the server, it is preferred to use ```conda``` to manage the Python virtual environment. Open the terminal on the Rstudio server and run the following commands. 

```conda create --name myenv python==3.7.6 ``` will create a virtual environment named “myenv” with 3.7.6 version Python on your home folder. It is ok to use other Python versions, however you will have to manually set up the environment variable “RETICULATE_PYTHON” for the reticulate package to find the correct Python. See the following section for more details. 

```conda env list ``` will check available virtual environments on your home folder.

```conda activate myenv``` will activate your virtual environments.

```pip install XXX``` will install package XXX into your virtual environment.

Check the online document https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html to learn more about managing virtual environment using ```conda```.

## Using Python virtual environment on the Rstudio Server

### Interactive mode 

Create a R file named config_Python_env.R in your working directory. Copy and past the following script to this file, and run this R script. By default, reticulate R package will use ```/opt/python/3.7.6/bin/python``` as the Python interpreter to execute Python script. When the Python version in your virtual environment is not 3.7.6, then it will have a big problem, some installed packages may not work. Therefore, it is recommended to set up the correct Python interpreter by yourself. By the way, your virtual environment will be installed in ```~/.conda/envs``` in your home directory.

```r

library(reticulate)

Sys.setenv(RETICULATE_PYTHON="~/.conda/envs/myenv/bin/python") #Python
use_python("~/.conda/envs/myenv/bin/python") # Which Python interpreter
use_condaenv("~/.conda/envs/myenv") # Which Python virtual environment
repl_python() # enter interactive mode for running Python script

```

### Batch mode 

It is suggested to run the Python script in batch mode on the terminal. You need first activate the Python virtual environment, then using Python interpreter to run the script in Batch mode.

```sh

conda activate myenv 
Python path_to_working_folder/your_python_script.py

```

## A working example 

In this section, we will go through an example, building classification ML models for the prediction of who was survived in the titanic data set, using Python on Rstudio server. 

* Create a working directory named titanic_prediction on your home folder and change the working directory to this folder on terminal.

    * ```mkdir titanic_prediction``` to create this folder

    * ```cd titanic_prediction``` to enter this folder 

    * ```mkdir data``` to create a data folder inside titanic_prediction

* Get the titanic data for the prediction task.

    * Get the code on the terminal ```cp /home/ysong1/test_dummyML/ 0_prepare_titanic_data.R .```, then run the R script to generate the titanic data. Please remember to set the working directory to the titanic_prediction folder before run the R script. 

    * Or directly copy the data on the terminal ```cp /home/ysong1/test_dummyML/data/titanic.csv ./data```

* Install the “dummyML” Python package, which is a Python package for automated data analysis in Python developed by Yipeng in Mengzhe’s team for doing automated data analysis using ML algorithms. 

    * ```sh

        conda create --name mlenv python==3.7.6

        conda activate mlenv        

        pip install dummyML

      ``` 

* Building ML models using dummyML package installed on your virtual environment

    * Get the script for testing dummyML package by ```cp /home/ysong1/test_dummyML/1_test_dummyML.py .``` on the terminal to copy the script to your project folder.

    * Activate the interactive mode using the R script in the section **Using Python virutal environment on Rstudio Server**. Change myenv to mlenv.

    * Run the script interactively and check the results.

