# Creating a Neural Network Predictive Model for Successful Fund Usage

## Overview

### Purpose
The purpose of this analysis is to create a neural network that can predict whether a fund application will ultimately be successful based on input features such as classification, fund usage, application type, income amount, and others.  The optimal solution would be a neural network model with an accuracy score above 0.75 when tested against the test data.  The neural network will be setup within the Keras framework in the Tensorflow library

## Results

### Control Model (from Deliverable 2)

#### Summary
This was the model used in deliverable 2 and is largely used as a performance baseline

#### Data Pre-processing
* Application Type and Classification were binned for application types less than 200 and classifications less than 1000
* Categorical data was one hot encoded
* All data was scaled using sci-kit learn's Standard Scaler

#### Neural Network Parameters
* Number of layers (including input and output layers): 3
* Number of nodes in each layer: 80, 30, 1
* Activation functions: rectified linear unit for first 2, sigmoid for final layer

#### Results
The accuracy score was 0.7262 and the loss was 0.5567

### Attempt #1 - More Neurons

#### Summary
This is largely the same design as the control, only with increased neurons in the first two layers

#### Data Pre-processing
* Application Type and Classification were binned for application types less than 200 and classifications less than 1000
* Categorical data was one hot encoded
* All data was scaled using sci-kit learn's Standard Scaler

#### Neural Network Parameters
* Number of layers (including input and output layers): 3
* Number of nodes in each layer: 160, 60, 1
* Activation functions: rectified linear unit for first 2, sigmoid for final layer

#### Results
The accuracy score was 0.7254 and the loss was 0.5658

### Attempt #2 - More Neurons and Additional Hidden Layer

#### Summary
This model builds off Attempt #1 and adds another hidden layer

#### Data Pre-processing
* Application Type and Classification were binned for application types less than 200 and classifications less than 1000
* Categorical data was one hot encoded
* All data was scaled using sci-kit learn's Standard Scaler

#### Neural Network Parameters
* Number of layers (including input and output layers): 4
* Number of nodes in each layer: 160, 60, 30, 1
* Activation functions: rectified linear unit for first 3, sigmoid for final layer

#### Results
The accuracy score was 0.7269 and the loss was 0.5639

### Attempt #3 - "The Kitchen Sink"

#### Summary
This model builds off Attempt #2 but adds additional data preprocessing, more layers, and more nodes in each layer.

#### Data Pre-processing
* Application Type and Classification were binned for application types less than 200 and classifications less than 1000
* Categorical data was one hot encoded
* All data was scaled using sci-kit learn's Standard Scaler
* Low correlating columns removed, specifically STATUS, SPECIAL_CONSIDERATIONS, and ASK_AMT

#### Neural Network Parameters
* Number of layers (including input and output layers): 6
* Number of nodes in each layer: 200, 100, 50, 25, 10, 1
* Activation functions: rectified linear unit for first 5, sigmoid for final layer

#### Results
The accuracy score was 0.7233 and the loss was 0.5682

## Summary

### Discussion of Results
The overall theme is that the neural network tuning of paramemters, specifically the number of neurons and number of layers, had no measurable effect at all on model performance.  Each of the four models had nearly the same accuracy score with very little difference.  Therefore it seems given this initial dataset, we won't be able to get a model above 0.75 accuracy using neural network tuning alone.

I did attempt some added data pre-processing.  I calculated the correlations between IS_SUCCESSFUL column (target column) with all the other feature columns.  I then squared those correlations and (admittedly un-intuitively) named the column "Bob".  Bob represents roughly the effect that each feature has on the target column.  A low Bob score close to zero would indicate that the column would not meaningfully predict IS_SUCCESSFUL either in the positive direction or the negative direction, whereas a Bob score closer to 1 would indicate a very strong correlation between a feature and the target column and therefore should be included in the training and test data for the neural networks.

![Bob Scores](https://raw.githubusercontent.com/jamesmedaugh/Neural_Network_Charity_Analysis/main/Screenshots/Bob.png "Bob Scores")

I found that "STATUS", "SPECIAL_CONSIDERATIONS_Y", "SPECIAL_CONSIDERATIONS_N" and "ASK_AMT" has relatively low Bob scores (as shown above), and so I decided to remove them from the final model in hopes that the removal of noisy columns would help the model focus more on the more strongly correlated features.  This did not seem to work as the data with the removed columns still yielded an accuracy score very close to the other 3 models.

### Final Recommendation
Though it is possible I could have done more given the input data to create a better model, my strong belief is that in order to create a better neural network for predicting charity success there needs to be more data collected and additional features need to be included.  I don't believe this dataset alone tells the whole story of what makes a charity successful.  Additionally, it may be the case that even with more data and more features we will not be able to create a better neural network as the predicatability of charity success is too difficult to predict and variable even with a very robust input data set.

