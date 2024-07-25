# Capstone Reflection
This small education project was created during my study in ML/AI during 1H 2024. As a quick reminder that the project consists of a Blackbox Bayesian Optimisation. There are eight unknown functions that should be maximized. I am only able to query each function once every few days. The inputs for each query are the following:
- Function one: 2-dimensional
- Function two: 2-dimensional
- Function three: 3-dimensional
- Function four: 4-dimensional
- Function five: 4-dimensional
- Function six: 5-dimensional
- Function seven: 6-dimensional
- Function eight: 8-dimensional

# The repository files overview
In the root directory, you can find the following files.
- 562_data.csv - This is the weekly response file for the project with returned values of 8th functions.
- leaderboards-2024.. A set of files with the current position of my solution against peers. 
- report. A small Python utility is to collect the data from individual functions and format it into report suitable for the submission via Web form.
- Files for each function are located in the corresponding subdirectories "\function_n". 

## For each function:
- bo1.ipynb - This is a main Python 3.11 code written in Jupyter Notebook to read the current known function data from excel file (data1.xlsx), visualize it, normalize it (if needed) and then run Bayesian Progress Regressor to propose the next point to request. It has also addtional methods to use during exploitation phase like linear-gradient ascent, quadratic ascend and plain brute force curve fitting. Please see code per in each function for details or read Approach section below for summary overview
- data1.xlsx - This is the main excel file to work with combined data with Notes. I record my request with columns X and entered column Y with response manually.
- initial_inputs.npy The Numpy matrix to load original inputs. It was used once to initialize data1.xlsx
- initial_outputs.npy The Numpy matrix to load original outputs. It was used once to initialize data1.xlsx
- first.ipynb The python script was used to initialize data1 file. It was used once to initialize data1.xlsx

## The Strategy
The authors of this training course, incredibly talented teachers from Imperial College in London, decided to avoid the dull training during all 25 weeks and announced a competition among their students from the middle of the course to find the maximum of unknown 8 functions in the range [0, 1). Moreover, the request for the function value and the response occurred rather slowly - once a week, which inspired students to use a strategy that made it possible to find a solution in a minimum number of attempts.

Similar problems were successfully solved during oil drilling in the last century with the Bayesian Optimization method which made the solution practical and interesting.
Unfortunately, due to personal reasons, I joined the competition in a month later, taking a losing position at the bottom of the leader boards, which inspire me to look for unconventional approaches.

**Firstly**, to quickly jump on the departing train, I used the solution from lesson #12 where I implemented the Gaussian Process Regressor with Radial Based Function. I had to adapt it to work in many dimensions.

**Secondly**, as requests are made relatively rarely, I consider that, it is important to conduct data analysis every time I receive a response per each function and be ready to change approach, code or method. 
For this purpose, I decided to use an Excel file with consolidated data that I received initially with the task and during weekly requests. In excel you can see the reflection of the Panda’s dataframe with requested (X columns) and result (Y column) as well as my notes per each requested record. 

In Notes you typically see the method which was applied (for example BO stands for Bayesian Optimization) and expected value as well as any other ideas and observations. The received data are entered in column Y manually.

To help with data analysis I visualize the plot per each $Y(x_i)$ function as well as 3d dimensional cube in the Python script to understand the distribution of maximum points and shape of modelling function.  

## Every time I received a response, I conduct data analysis:
- Did the received response correspond to the expected value? The big difference means I have to improve the method, code or change the approach
- Were the previous request/response values changed? This way, I discovered that the functions 2, 3, 6 have the noise and by accumulating the response value it was quite easy to model this noise by White Kernel coupled with RBF for Gaussian Regressor 
- During the computation of bo scripts, I checked the convergence of optimization functions and applied known workarounds if I received warnings (see the code).
- I used a simple grid to find the maximum of acquiring function but it happens to work badly in many dimensions. Indeed 10 points of grid gives 10^8 points in 8 dimension which is a limit for my workstation. Instead of dense grid I use a optimize.minimize function from SciPy to find the maximum point and value with a good precision.
- The Bayesian Optimization is a relatively new method for me, so I feel it is a great for function exploring but I developed the old proven linear gradient method for function exploitation. As I could not get derivative for the gradient under terms of task, I approximate the function by liner hyperplane near known maximum point by selecting the required nearest points.
- I developed a similar approach with quadratic hyperplane approximation near the maximum point and I was very lucky to find out that function 8 is a quadratic polynomial itself, so I quickly recovered the function, maximum point and value.
- I check the leader boards table to understand my relative position against peers which help me select either “exploration or exploitation” mode and method.

## Lessons Learned
-	I saw that a Gaussian Progress Regressor is struggling to find the optimal function for kernel by generating a warning messages. During problem solving, I came to conclusion that appropriate scaling is very important to get a good results. The scaling should be done at the very beginning.
-	There is a joke that “Every code has a bug”, so debugging and critical code review is very important from the very beginning.
-	Surrogate function does not play such important role as it might seem. The Matern kernel was excessive because RBF is enough.
-	Linear plain approximation sometimes does not work due to multicollinearity of nearest points. I have to conduct appropriate analysis from the very beginning too.
-	The requirement to place the code and data in the github should be given as a task guidance at the very beginning of competition rather than at the end. It helps to track the version of code.
-	I wish I had known the botorch library from the very beginning to apply BO in a trusted region! 

## How can I use this experience in the future?
I become comfortable with the Bayesian Optimization and I believe this is a good approach in finding the optimal hyperparameters in the Neural Network. I have ongoing project to create the AI assistant for IT technical support team where BO will be very useful to train Neural Network.

Generally, I liked the project because it was fun. 

A Big Thank You to my teachers, Carleton and Matilde.

Andrey, the summer 2024.
