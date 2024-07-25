# capstone
This small education project was created during my study in ML/AI. 
A quick reminder that the project consists of a Bayesian Optimisation competition. There are eight unknown functions that should be maximized. I am only able to query each function once every few days. The inputs for each query are the following:
- Function one: 2-dimensional
- Function two: 2-dimensional
- Function three: 3-dimensional
- Function four: 4-dimensional
- Function five: 4-dimensional
- Function six: 5-dimensional
- Function seven: 6-dimensional
- Function eight: 8-dimensional

In the root directory, you can find the following files.
- 562_data.csv - This is the weekly response file for the project
- leaderboards-2024.. A set of files shows the current position of the current solution against competitors. This information might be useful in understanding which mode of optimization to use. At the bottom of the table, it makes sense to use "exploration" mode rather than "exploitation"
- report. A small Python utility is used to collect the last records from data files per each function and format one report suitable for the quick entry into the Web form.

## For each function
Files for each function sit in the corresponding directory "\function_n". There, you can find a few files.
- bo1.ipynb - This is a Python 3.11 code written in Jupyter Notebook and has the main code to read the current data from excel file data1.xlsx, visualize it, apply normalization, where it makes sense, and then run Bayesian Progress Regressor to propose the next point to request as well as it has any other methods like linear-gradient ascent, quadratic approximation to find max or brute force curve fitting. Please see code per each function for details
- data1.xlsx - This is the main excel file to work with data. There you can see the Pandas dataframe with original data and data which were requested (X columns) with result (Y column) as well as Notes per each requested record. In Notes you typically see the method which was applied (BO stands for Bayesian Optimization) and expected value as well as any other ideas and observations. The received data are entered in column Y manually and will be available for processing by bo1 script
- initial_inputs.npy The Numpy matrix to load original inputs. It was used once to initialize data1.xlsx
- initial_outputs.npy The Numpy matrix to load original outputs. It was used once to initialize data1.xlsx
- first.ipynb The python script was used to initialize data1 file

## The general approach
As requests are made relatively rarely, I consider that, it is important to conduct data analysis every time I receive a response per each function. Specifically, I checked:
- Did the received response correspond to the expected value? The big difference means I have to improve the method code or change the method itself
- Were the previous request/response values changed? This way, I discovered that the functions 2, 3, 6 have the noise and by accumulating the response value it was quite easy to model this noise by White Kernel coupled with RBF for Gaussian Regressor 
- I visualize the plot per each Y(Xi) function as well as 3d dimensional cube to understand the distribution of maximum points and shape of modelling function.
  
During the computation of bo1 scripts, I checked the convergence of optimization functions and applied known workarounds (see the code) if I received warnings.

For the gradient ascend, I developed metrics to check the colinearity of underlying data points.

For function 8, I was lucky to identify that all points sit on quadratic polynomial, so I quickly recovered the function, maximum point and value.

Generally, I liked the project because it was fun.

Thank you to my teachers, Carleton and Matilda.

Andrey, the summer 2024.
