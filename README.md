# Bike-sharing system demand estimation
> BoomBikes aspires to understand the demand for shared bikes among the people after the ongoing quarantine situation ends across the nation due to Covid-19. They have planned this to prepare themselves to cater to the people's needs once the situation gets better all around and stand out from other service providers and make huge profits.They have contracted a consulting company to understand the factors on which the demand for these shared bikes depends. Specifically, they want to understand the factors affecting the demand for these shared bikes in the American market.

  Essentially, the company wants —
  > Which variables are significant in predicting the demand for shared bikes.
  > How well those variables describe the bike demands.
  The project is to build a linear regression model to predict the bike demand based on various x-parameters provided


## Table of Contents
* [General Info](#general-information)
* [Technologies Used](#technologies-used)
* [Conclusions](#conclusions)
* [Acknowledgements](#acknowledgements)

<!-- You can include any other section that is pertinent to your problem -->

## General Information
- Project was developed in python version 3.11.7 | packaged by Anaconda, Inc..
- A US bike-sharing provider BoomBikes has recently suffered considerable dips in their revenues due to the ongoing Corona pandemic. The company is finding it very difficult to sustain in    
  the current market scenario. So, it has decided to come up with a mindful business plan to be able to accelerate its revenue as soon as the ongoing lockdown comes to an end, and the 
  economy restores to a healthy state.  
- BoomBikes aspires to understand the demand for shared bikes among the people after the ongoing quarantine situation ends across the nation due to Covid-19.
- The dataset being used is provided by a consulting company contracted by BoomBikes to understand the factors on which the demand for these shared bikes depends

## Technologies Used

Python version ::  3.11.7 | packaged by Anaconda, Inc.

Libraries used in the environment are ::<br>
Numpy           ::  1.26.4<br>
Pandas          ::  2.1.4<br>
Matplotlib      ::  3.8.0<br>
Seaborn         ::  0.11.0<br>
Sklearn         ::  1.2.2<br>
Statsmodels     ::  0.14.0<br>

## Conclusions ::

## Step 1: Reading and Understanding the Data
	- There are no null values in the data set
	- Dataset has categorical representatives, with some of them at multiple levels. Hence dummy varibales are to be generated
	- Dataset has different column values at different ranges. Hence scaling is required before model building

## Step 2: Visualising the Data
	- Temp and aTemp are clearly linearly related. Hence one of these could be potentially insignificant for model building
	- Target y i.e. cnt is observed to have monotinic relation with temp. A good indication for a linear regression
	- Target y i.e. cnt doesnt seem to have clear relation with Humidity and windspeed. Need to check if combination of these with other parameters have any 
          significance in model building
	- Categorical variables observed are Year, Month, Season, Weekday,Workingday,Holiday,Weather situation
	- Except working day and week day, all other variables show good impact on the target y-variable cnt
	- Given that cnt is a sum of casual and registered user count, these sub components of cnt were also plotted
	- Observed an opposite behavior of the casual and registered users w.r.t "working day" and "week day", which is resulting
  	  in a weak relation of these two parameters to target y. Hence, we cannot ignore these parameters because if it happens to be a situation where casual to 
           registered users usage  
          ratio is significantly away from 0.5, then the working day and week day will show a clear impact on cnt. 
	- Hence it is ideal to build 2 linear regression models, one to capture the casual user behavior & the other to capture the registered user behavior. 
          Finally, overall demand is a sum of these 2 behaviors. 
	  However, first attempt is to build a linear regression for target y which is cnt

## Step 3: Data Preparation
	- Number of columns in original input data ::  16
	- 4 columns are removed from the input data as these are not features ::  ['instant', 'dteday', 'casual', 'registered']
	- Total number of columns after adding the dummy variables to the cleaned input data ::  30

## Step 4: Splitting the Data into Training and Testing Sets
  	 - After rescaling, all parameters are now within 0 to 1
 	  - There could be a strong relation between Temp and aTemp
 	  - Fair relation exists between "Fall" and "Temp", "cnt" and "Temp", "cnt" and "Fall"
 	  - Seasons are correlated to months. Example summer has good correlation with "April" and "May" months. Winter to "Oct & Nov"
  	 - Workingday is strongly INVERSELY correlated to Sunday as a white patch can be observed in above heat map. 
 	  - Mist and cloudy is correlated to humidity 
  	 - All these dependecies can be taken care using "Variance Inflation Factor (VIF)" and "Recursive Feature Elimination RFE" to finalize on variable 
           selection for model building
	
## Step 5: Building a linear regression model using "Statsmodels" package
	- R2 is 41% which means "Temp" parameter alone could explain 41% variance in the target y cnt
	- P = 0 ==> chosen x variable is significant as the null hypothesis (x is insignificant) failed
	- Lower value of Prob (F-statistic) indicate that the model is significant
	- R2 is 85% after adding all the parameters
	- Observing the p-values, it looks like some of the variables aren't really significant (in the presence of other variables).
        - VIF was used to understand the multicollinearity

## Step 6: Recursive Feature Elimination RFE and re-building of model using statsmodel
	- RFE shortlisted 10 parameters as significant parameters for model development
	- humidity and temperature are observed to have VIF > 5 i.e there is scope to eliminate these parameters from model
	- RFE shortlisted parameters resulted in 83% R2 model. It saved a lot of time in shortlisting the parameters
	- Since the VIF of these shortlisted parameters is in the range 0 to 10, an attempt to reduce the number of independent variables was done
	- Just by removing humidity, the model R2 has gone down from 83% to 80%. However, the p value of all the remaining parameters is now very close to 0 which 
          indicates that these are significant parameters
	- Also the VIF of final set of parameters ranges between 1 to 5, which means that these are fairly independent / weakly dependent parameter
	- Finally the Prob (F-statistic) is 2.24e-171 which is very close to 0. This indicates that the model is significant

## Step 7: Residual Analysis of the train data
	- Clearly the error data set for the train data is observed to follow normal distribution. Also, there is no trend of error data w.r.t fitted values. 
          Hence the linear regression assumption is verified

## Step 8: Making Predictions Using the Final Model
	- Scaling is performed without fit. This is why we see that some of the columns have range outside of [0 1]. 
	- Error on ytest also follows normal distribution same as that of train set

## Step 9: Model Evaluation
	- R2 on the test data is 77% which is very close to R2 on train data which is 80.5%
	- Hence the fit is generalizing well across the population


## Acknowledgements
Give credit here.
- This project was inspired by UPGrad...
- Upgrad lecture notes and video classes were helpful


## Contact
Created by [@githubusername] - feel free to contact me!


<!-- Optional -->
<!-- ## License -->
<!-- This project is open source and available under the [... License](). -->

<!-- You don't have to include all sections - just the one's relevant to your project -->
