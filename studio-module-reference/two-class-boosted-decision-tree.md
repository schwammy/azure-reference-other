---
title: "Two-Class Boosted Decision Tree | Microsoft Docs"
ms.custom: ""
ms.date: 12/18/2017
ms.reviewer: ""
ms.service: "machine-learning"
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "reference"
ms.assetid: e3c522f8-53d9-4829-8ea4-5c6a6b75330c
caps.latest.revision: 24
author: "jeannt"
ms.author: "jeannt"
manager: "cgronlund"
---
# Two-Class Boosted Decision Tree
*Creates a binary classifier using a boosted decision tree algorithm*  
  
 Category: [Machine Learning / Initialize Model / Classification](machine-learning-initialize-model-classification.md)  
  
##  <a name="Remarks"></a> Module Overview  

This article describes how to use the **Two-Class Boosted Decision Tree** module in Azure Machine Learning Studio to create a machine learning model that is based on the boosted decision trees algorithm. 

A boosted decision tree is an ensemble learning method in which the second tree corrects for the errors of the first tree, the third tree corrects for the errors of the first and second trees, and so forth.  Predictions are based on the entire ensemble of trees together that makes the prediction.
  
Generally, when properly configured, boosted decision trees are the easiest methods with which to get top performance on a wide variety of machine learning tasks. However, they are also one of the more memory-intensive learners, and the current implementation holds everything in memory. Therefore, a boosted decision tree model might not be able to process the very large datasets that some linear learners can handle.
  
For more information about how to choose an algorithm, see these resources:  
  
-   [Machine learning algorithm cheat sheet for Microsoft Azure Machine Learning Studio](https://docs.microsoft.com/azure/machine-learning/studio/algorithm-cheat-sheet)  
  
-   [How to choose Azure Machine Learning algorithms for clustering, classification, or regression](https://docs.microsoft.com/azure/,machine-learning/machine-learning-algorithm-choice/)  
  
This module creates a classification model, which is a supervised learning method. Therefore, your training data must be a *tagged dataset* that includes a label column with a value for all rows.
  
After preparing the training data, you can train the model by connecting the boosted decision tree model to the [Train Model](train-model.md) or [Tune Model Hyperparameters](tune-model-hyperparameters.md) modules. Both of these training modules create a trained model that can be used for prediction.  
  
## How to Configure a Boosted Tree Model  
  
1.  In Azure Machine Learning Studio, add the **Boosted  Decision Tree** module to your experiment.
  
2.  Specify how you want the model to be trained, by setting the **Create trainer mode** option.
  
    + **Single Parameter**. If you know how you want to configure the model, you can provide a specific set of values as arguments.
  
    + **Parameter Range**. If you are not sure of the best parameters, you can find the optimal parameters by using the [Tune Model Hyperparameters](tune-model-hyperparameters.md) module. You provide some range of values, and the trainer iterates over multiple combinations of the settings to determine the combination of values that produces the best result.
  
3.  For **Maximum number of leaves per tree**, indicate the maximum number of terminal nodes (leaves) that can be created in any tree.
  
     By increasing this value, you potentially increase the size of the tree and get better precision, at the risk of overfitting and longer training time.
  
4.  For **Minimum number of samples per leaf node**, indicate the number of cases required to create any terminal node (leaf) in a tree.  
  
     By increasing this value, you increase the threshold for creating new rules. For example, with the default value of 1, even a single case can cause a new rule to be created. If you increase the value to 5, the training data would have to contain at least 5 cases that meet the same conditions.
  
5.  For **Learning rate**, type a number between 0 and 1 that defines the step size while learning.  
  
     The learning rate determines how fast or slow the learner converges on the optimal solution. If the step size is too big, you might overshoot the optimal solution. If the step size is too small, training takes longer to converge on the best solution.
  
6.  For **Number of trees constructed**, indicate the total number of decision trees to create in the ensemble. By creating more decision trees, you can potentially get better coverage, but training time will increase.
  
     This value also controls the number of trees displayed when visualizing the trained model. if you want to see or print a single tree, set the value to 1. However, when you do so, only one tree is produced (the tree with the initial set of parameters) and no further iterations are performed.
  
7.  For **Random number seed**, optionally type a non-negative integer to use as the random seed value. Specifying a seed ensures reproducibility across runs that have the same data and parameters.  
  
     The random seed is set by default to 0, which means the initial seed value is obtained from the system clock.  Successive runs using a random seed can have different results.
  
8.  Select **Allow unknown categorical levels** option to create a group for unknown values in the training and validation sets.  
  
     If you deselect this option, the model can accept only the values that are contained in the training data.
     
     If you allow unknown values, the model might be less precise for known values, but likely can provide better predictions for new (unknown) values.
  
9. Train the model.
  
    + If you set **Create trainer mode** to **Single Parameter**, connect a tagged dataset and the [Train Model](train-model.md) module.  
  
    + If you set **Create trainer mode** to **Parameter Range**, connect a tagged dataset and train the model by using [Tune Model Hyperparameters](tune-model-hyperparameters.md).  
  
    > [!NOTE]
    >  + If you pass a parameter range to [Train Model](train-model.md), it uses only the first value in the parameter range list.  
    > + If you pass a single set of parameter values to the [Tune Model Hyperparameters](tune-model-hyperparameters.md) module, when it expects a range of settings for each parameter, it ignores the values, and uses the default values for the learner.  
    > + If you select the **Parameter Range** option and enter a single value for any parameter, that single value you specified is used throughout the sweep, even if other parameters change across a range of values.
  
### Results

After model training is complete, right-click the output of [Train Model](train-model.md) or [Tune Model Hyperparameters](tune-model-hyperparameters.md) to view the results:

+  To see the tree that was created on each iteration, select **Visualize**. 
+ To drill down into the splits and see the rules for each node, click each tree.
  
## Examples

Explore the [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) to see examples of how boosted decision trees are used in machine learning:
  
-   The [Direct marketing](http://go.microsoft.com/fwlink/?LinkId=525168) sample uses the **Two-Class Boosted Decision Tree** algorithm to predict customer appetency.
  
-   The [Flight delay prediction](https://gallery.azureml.net/Experiment/837e2095ce784f1ba5ac623a60232027) sample uses the **Two-Class Boosted Decision Tree** algorithm to determine whether a flight is likely to be delayed.  
  
-   The [Credit card risk](http://go.microsoft.com/fwlink/?LinkId=525270) sample uses the **Two-Class Boosted Decision Tree** algorithm to predict risk.  
  
## Technical Notes  

This section contains implementation details and frequently asked questions.

### Guidance

+ To train a boosted decision tree model, you must provide multiple data instances. An error is generated during the training process if the dataset contains too few rows.  

+ If your data has missing values, you must add indicators for the features.
  
+ In general, boosted decision trees yield better results when features are somewhat related. If features have a large degree of entropy (that is, they are not related), they share little or no mutual information, and ordering them in a tree will not yield a lot of predictive significance. If this is not the case, you might try a random forests model.
  
    Boosting also works well when you have many more examples than features because the model is prone to overfitting.

+ Do not normalize the dataset. Because the treatment of features is a simple, non-parametric, less-than or greater-than comparison, normalization or any form of non-monotonic transformation function will have little effect. 

+ Features are discretized and binned prior to training, so only a relatively small set of threshold candidates are considered, even for continuous features.

### Technical details

For detailed information about the boosted decision tree algorithm, see [Greedy Function Approximation: A Gradient Boosting Machines](http://www-stat.stanford.edu/~jhf/ftp/trebst.pdf).  
  
The boosted decision tree algorithm in Azure Machine Learning uses the following boosting method:
  
1.  Start with an empty ensemble of weak learners.
  
2.  For each training example, get the current output of the ensemble. This is the sum of the outputs of all weak learners in the ensemble.
  
3.  Calculate the gradient of the loss function for each example.

    This depends on whether the task is a binary classification problem or a regression problem.  
  
    + In a binary classification model, the log-loss is used, much like in logistic regression.  
  
    + In a [regression](boosted-decision-tree-regression.md) model, the squared loss is used, and the gradient is the current output, minus the target).
  
4.  Use the example to fit a weak learner by using that gradient as the target function.
  
5.  Add that weak learner to the ensemble with a strength indicated by the learning rate, and if desired, go to Step 2.  
  
    The weak learners in this implementation are the least-squares regression trees, which use the gradients calculated in Step 3 as the target. The trees are subject to the following restrictions:  
  
    + They are trained up to a maximum number of leaves.  
  
    + Each leaf has a minimum number of examples to guard against overfitting.  
  
    + Each decision node is a single feature that is compared against some threshold. If that feature is less than or equal to the threshold, it goes down one path, and if it is greater than the threshold, it goes down the other path.  
    + Each leaf node is a constant value.  
  
6. The tree-building algorithm greedily selects the feature and threshold for which a split will most decrease the squared loss with regard to the gradient calculated in Step 3. It is subject to a minimum number of training examples per leaf. 

    It repeatedly splits until it reaches the maximum number of leaves, or until no valid split is available. 

##  <a name="parameters"></a> Module Parameters  
  
|Name|Range|Type|Default|Description|  
|----------|-----------|----------|-------------|-----------------|  
|Maximum number of leaves per tree|>=1|Integer|20|Specify the maximum number of leaves allowed per tree|  
|Minimum number of samples per leaf node|>=1|Integer|10|Specify the minimum number of cases required to form a leaf|  
|Learning rate|[double.Epsilon;1.0]|Float|0.2|Specify the initial learning rate|  
|Number of trees constructed|>=1|Integer|100|Specify the maximum number of trees that can be created during training|  
|Random number seed|Any|Integer||Type a value to seed the random number generator that is used by the model. Leave it blank for the default.|  
|Allow unknown categorical levels|Any|Boolean|True|If True, an additional level is created for each categorical column. Any levels in the test dataset that are not available in the training dataset are mapped to this additional level.|  
  
##  <a name="Outputs"></a> Output  
  
|Name|Type|Description|  
|----------|----------|-----------------|  
|Untrained model|[ILearner interface](ilearner-interface.md)|An untrained binary classification model|  
  
## See Also  
 [Classification](machine-learning-initialize-model-classification.md)   
 [Boosted Decision Tree Regression](boosted-decision-tree-regression.md)   
 [A-Z Module List](a-z-module-list.md)