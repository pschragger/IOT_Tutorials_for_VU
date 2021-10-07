# Machine Learning on Node Red Support #

Need to pick a node-red to use: your laptop or pi.  I used my lap-top but if you do not or cannot use python3 on your laptop then you should use your pi as your platform.

Note that the node-red you are using is determined by the URL you are using to access it.

For a quick implementation of ML we are going to use the [node-red-contrib-machine-learning-v2](https://flows.nodered.org/node/node-red-contrib-machine-learning-v2) package.
So in node-red go to "Manage Palette" function and click on the Install button next to the module name.

This should now show that it is installed on your node-red instance.

Note the red-node ml nodes require that you have:

* Python 3.6.4 or higher accessible by the command 'python' (on linux 'python3')
* Numpy
* Pandas
* SciKit-Learn
* Tensorflow (optional: can be skipped)

To be sure you have a valid installation check the versions and installations using the following instructions

## Check for python ##

In a command window or ssh session 

1. To see if you have python3 installed run

command line:  python --version

result: Python 3.7.6

2. If you a good python version then skip to "Check python modules" section;
otherwise you have to load it python and that  will depend on your system.
Follow the instructions for your OS. Be sure to find a version that will work on your version of OS.

   * windows  - https://www.python.org/downloads/windows/
   * mac - https://www.python.org/downloads/macos/
   * linux - https://www.python.org/downloads/source/

3. If you have python  installed but it is less then 3.6.4 (2.6 or 3.5) you will need to upgrade if your OS can handle the latest  If not then you should use the PI as your platform for development of Machine Learning.

## Check python modules ##

1. Once you are sure you have reasonable version of python installed. You should test to see if each package is installed.

command line: python

python >>> import numpy

python >>> import pandas

python >>> import sklearn

python >>> import tensorflow

These should not fail if the packages are installed on your machine
If successful you are done  and can continue the node red test
   else:

2. if they fail then  run

> pip install --upgrade pip

> pip install numpy

> pip install pandas

> pip  install sklearn

> pip  install tensorflow

If successful go back to step 1 in the "Check python modules" section.

3. Otherwise first Test to see if pip is installed using :

> command line:  pip install --upgrade pip

If you do not have pip at all then 
use: https://pip.pypa.io/en/stable/installation/  as the instruction to load pip into your computer. The get-pip.py program is the simplest to use.
Once pip is installed go back to the install step 2 in this "Check python modules" section.

## Setup Data ##

Once the python environment is setup you will need to get the training data loaded locally.

1. cd to  a local directory that has a path without any spaces in the names and create

>Command line: mkdir test

>Command line: cd test

>Command line: mkdir models

2. Copy the data and test files into the test directory

*[iris.data](./iris.data) - training data with data from https://archive.ics.uci.edu/ml/datasets/iris

*[test1.json](./test1.json) - test1 

*[test2.json](./test2.json) - test2

*[multitest.json](./multitest.json) - multiple datasets in a single json message

## Edit the flows for ML ##

I have put the example flows export in [nodered_ml_flows.json](http:./nodered_ml_flows.json)

1. Import that file or cut and paste the content in the nodered import page.
2. Configure the "create dataset" node by updating:
   a. Path - the data file absolute path - example in node - /users/pschragger/test/iris.data
   b. Save folder - where the dataset is stored - example in node - /users/pschragger/test/datasets
3. Configure the "Dataset folder" path in all of the "load dataset" nodes to match 2.b.

## Create the model ##
1. Run the "create dataset" flow
2. Run the "decision tree classifier trainer" flow
3. test using the "assessment" flow

## Use mqtt to send data to the decision tree classifier and see its responses ##

1. Be sure you mqtt broker ( mosquitto )  is running or start it.

2. Add the mqtt broker you setup in the mqtt input node "classification"
and mqtt output node "predictions", usually that means you create localhost configuration.

3. Start a mosquitto subscript of the "predictions" topic
ou installed the test data 

In a command window run:

> mosquitto_sub -h localhost -t predictions

4. Publish the test jsons to the classification topic in a different command windw

You will need to change the directory path to wherever you installed the data.
For example I ran mosquitto_pub using:

>mosquitto_pub -h localhost -t classification -f /Users/pschragger/test/test1.json

>mosquitto_pub -h localhost -t classification -f /Users/pschragger/test/test2.json

>mosquitto_pub -h localhost -t classification -f /Users/pschragger/test/multitest..json

## Results ##

We  seen how to setup the machine learning module in nodered.
From this we can see that the "training dataset" for a descision tree requires
rows containing important data and the expected results ( the classifcation made by an experte
).
For the live test only the important data was sent and the decision tree model
decided on the result.



