# SC1015 Introduction to Data Science and AI: Mini-project - Android Malware Detection

School of Computer Science and Engineering
Nanyang Technological University
Lab: z137
Group: 2

Members:
1. Glenda Yap Kah Heng (@glendaykh)
2. Tessa Tan Jie Ru (@tessatan_)
3. Tyler S'ng QiWei (@tyl_sng)

# Description:

This repository contains all the Jupyter Notebooks, datasets, video presentations, and the source materials/references we have used and created as part of the Mini-project for SC1015: Introduction to Data Science and AI.

This README briefly highlights what we have accomplished in this project. For a more detailed explanation of things, please refer to the the Jupyter Notebooks in this repository. 

# Table of Contents:

1. Problem Formulation
2. Data Preparation and Cleaning
3. Exploratory Data Analysis
4. Dimensionality Reduction
5. Machine Learning
6. Conclusion





# 1. [Problem Formulation](../SC1015-mini-project/)

**Background**
The spread of malware is a growing issue with severe consequences. According to Kaspersky Security Network, in Q3 2022, a total of 5,623,670 mobile malware, adware, and riskware attacks were blocked. Thus, in order to investigate our cybersecurity, we decided to analyse the capabilities of android malware detection models.

**Motivation**

When it comes to preventing malware attacks, there are multiple different vantage points in which we can tackle the issue from. For this project, we have chosen to focus specifically on leveraging network data to detect malware. Why did we choose this approach? Well, malware often communicates with command-and-control servers over the network, and this communication can be captured in network flow data. We discovered that by analyzing network flow data and detecting patterns, we are able to identify the suspiciousness of an application, which aids in malware detection.

The objective of our project is two-fold. Firstly, we aim to achieve good accuracy in detecting malware from samples of benign and malware applications using 2 approaches. We began by selecting features to analyse based on their definitions, and proceeded to use dimension reduction to further improve this analysis. Dimension reduction aids with simplifying and optimising data analysis and machine learning algorithms. Secondly, we compared features/characteristics of different data types to provide recommendations on the best strategy for malware detection. 

**Our question:** What are the variables that best predict the type of malware attack that happened on an Android device.

**Our dataset:** [Android Malware Detection on Kaggle (updated-version Feb 2023)](https://www.kaggle.com/datasets/subhajournal/android-malware-detection?select=Android_Malware.csv)

The database for this project was extracted from Kaggle. It has 355630 Data points and it contains features of network flow characteristics and statistics. The data is split into four categories, software labelled ‘Android adware’, ‘Android Scareware’, ‘Android SMS Malware’ and ‘Benign’. These will be the response variables we will be examining, where the remaining variables in our dataset will act as listed predictors that we will be using in this study.






# 2. [Data Preparation and Cleaning](../SC1015-mini-project/)
  In this section of the project, we prepared and cleaned the kaggle dataset to be used in (1) EDA and (2) Dimension reduction in the later sections. We performed the following:

  **1. Preliminary Feature Selection:**
  
  21 relevant variables out of 85 were selected, which can be categorised into 4 types:
  * Total Length of Fwd Packets and Total Length of Bwd Packets: These features represent the total length of data transmitted in the forward and backward directions during a flow. Similar to the total packet counts, anomalously high volumes of data transmission can be indicative of malware activity.
  * Fwd Packet Length Max, Fwd Packet Length Mean, Fwd Packet Length Std, Bwd Packet Length Max, Bwd Packet Length Mean, and Bwd Packet Length Std: 
  These features represent various statistics of packet length for both the forward and backward directions during a flow. Malware may use specific packet length patterns to hide its communication, which can be identified through these statistics. For example, high standard deviations in packet length can be an indicator of encrypted traffic, which may be used by malware to hide its communication.
  * Flow Bytes/s and Flow Packets/s: These features represent the rate of bytes and packets transmitted per second during a flow. High rates can indicate potential malware activity.
  * FIN Flag Count, SYN Flag Count, RST Flag Count, PSH Flag Count, ACK Flag Count, and URG Flag Count: These features represent the count of different TCP flags set during a flow. Some types of malware may use specific TCP flag combinations for their communication.

  **2. Splitting the dataset in the different type of possible malware data**
  
  For the purpose of our analysis, we split our data into 4 frames depending on the type of malware attacks relating to: Android_Adware attacks, Android_Scareware attacks, Android_SMS_Malware attacks, with Benign attacks. Further data cleaning and preparation was done separately for all data frames including removal of NaN and infinite values. The respective frames containing data relating to malware attacks were then merged with the Benign Attack data frame to act as a control.

  Due to the disproportionate amount of data when comparing the Benign to the other types of Malware Attacks, we will be controlling the volume and ratio of the resulting merged Datasets with random resampling processs which will be utilized for EDA/Machine Learning.
      
  **3. Encoding categorical data (label)**
  
  The categorical variable that we are trying to predict is “Label” - which consists of the 4 different types of android attacks. In order to do any form of analysis on this, we must encode it. For this, we use LabelEncoder from the sklearn.preprocessing library.
  
  **4. Conversion of final cleaned and prepared dataframes to pickle file**
  
  The final step of data cleaning is to convert the various data frames that we will be using for EDA and for the machine learning techniques.
 
 
 
 
 
 
 
# 3. [Exploratory Data Analysis and Visualisation: Training the models and Predicting Test Data](../SC1015-mini-project/)
  
  In this section we will do general EDA to gather relevant insights. Due to the different nature of the data, (14 Numerical Columns for the Packets variables and 6 Categorical Data for the Flag variables), we will be breaking it into two parts where we will explore the best method to explore and visualize the different data types for relevant insights.

  **Down-sampling using resample from sklearn**

  This is done to reduce the over density of the data given the volume of the rows within the dataframe. The downsampling operation is performed in two steps. 
  In the first step, the majority and minority classes are separated into two different dataframes. The majority class is then downsampled by randomly selecting a subset of the samples without replacement to match the number of samples in the minority class. 
  In the second step, the balanced data set obtained from the first step is further downsampled. Both the majority and minority classes are divided into two different dataframes and downsampled independently.
  Finally, the downsampled majority and minority classes are concatenated to obtain the final balanced dataset. The resulting dataset has an equal number of samples for each class.

  The resample process reduced the sheer volume in terms of row for the different DataFrames that will be visualized in the EDA process below in order to help with better visual clarity for method chosen.

  **3.1.1 Exploratory Data Analysis (Numerical)**
  
  In this case, we have chosen boxplots and stripplots to best showcase our results for general distribution to best visualize the downsampled data.

  The .describe() function is also utilized on the raw data is to showcase the different (Numerical) variables for the types of Malware Attacks vs Benign.
      
  **3.1.2 Insights of Exploratory Data Analysis (Numerical)**
  
  To preface we must acknowledge that using graphical representation for such a large volume of (Numerical) data against (Categorical) has its downsides of poor visual clarity even after downscaling of the rows within the DataFrames has been done:
  
  * As shown in the boxplot and stripplot, there are many outliers in the values, however the value that standout the most visually would be the ['Bwd Packet Length max'] for SMS Malware: Where the outliers have a large spread which is also reflected in their std value.
  * Significant observation difference of around 40% to 30% from the (mean) value result of .compare() function on the .describe() results of the raw Numerical Dataframes for the types of Malware Attacks vs Benign
  * Though the boxplots visually show a large amount of outliers reflected throughout all the (Numerical) data, we will not remove them as they are important for anomaly detection. 
     
  These insights will be further developed into a potential hypothesis/insights in the prediction accuracy when doing machine learning model stage. Such as the feature importance rankings, we can take a look back at the Significant observation difference later on in Part 4: Machine Learning

  **3.2.1 Exploratory Data Analysis (Categorical)**
  
  For each of the 6 flags, we created countplots for visualisation. 
    
  **3.2.2 Insights of Exploratory Data Analysis (Categorical)**
  
  Due to the binary nature of the data variables where the values are '1' or '0' for all the flags as well as the categorical ['Label'], we are able to utilise the countplot function in seaborn to best visualise the categorical data. Similar to numerical data, we must acknowledge that using graphical representation for such a large volume of data has its downsides in terms of visual representation. Our key insight is that the RST flag count for all the different types of Malware attacks including Benign can be omitted in the machine learning model stage as it is always not triggered irregardless of the ['Label'].
    
   
   
   
   
   
    
# 4. [Dimension Reduction](../SC1015-mini-project/)
  As we have seen in *“Part 1: Data Cleaning and Preparation”*, our dataset has a high dimension: there are a total of 21 columns left even after Preliminary Feature Selection. In this section we shall be exploring dimension reduction for both the (Numerical) and (Categorical) data of the datasets.

  For the (Numerical Variables), dimension reduction will be achieved using Principal Component Analysis (PCA), which is the general convention for continuous variables. However for categorical data, Non-negative Matrix Factorization (MCA) is used instead. This is because PCA is often used for linear dimension reduction and finding the most important features in a dataset, while NMF is useful for identifying patterns in non-negative data and can produce sparse and interpretable factorizations.
  
  **4.1.1 Dimension Reduction using PCA (Numerical)**

  PCA is a technique reducing the number of variables in a dataset while retaining as much information as possible. It identifies the directions in which the data varies the most and projects the data onto a new coordinate system defined by these directions, called principal components (PC). This allows for the reduction of the dimensionality of the data while preserving most of its variability.

  The resulting PCA score represents the proportion of the total variance in the data that is explained by the two PC. In this case, since we reduced the dimensionality of the data to 2 dimensions, the score represents the sum of the explained variance ratios of the two PC. The score ranges from 0 to 1, where 1 indicates that all of the variance in the original data is explained by the principal components. A score of 0 indicates that none of the variance is explained by the principal components. In general, a higher score indicates that the PC are able to capture more of the variance in the original data.

  For instance, a PCA score of around 0.4 ~ 0.6 suggests that the two principal components are able to capture about 40% ~ 60% of the variance in the original data. This means that the two PC may not be able to fully capture the patterns in the data, and there may be additional dimensions that are important for explaining the variance in the data.
  
  At this stage we can select carefully the higher PCA score which indicates that the PC are able to capture more of the variance in the original data. Up till this point for each type of Malware attack, we have decided to either use PCA for a General Reduction in dimension without and preliminary selecting of specific feature types as well as segmenting it into Forward Packets, Backward Packets and Flow.

   **4.1.2 Selecting optimal n-component score (Numerical)**

  Here are our key insights: From the 14 variables we have reduced the dimensions to 8 total variables. The General Reduction can be good control therefore it will be used, however we will include the segmented dimension reduced variables to help us better answer the question since the [What are the variables that help predict the type of malware attack that happened on an Android device].

   **4.2.1 Dimension Reduction using NMF (Categorical)**

On the other hand, MCA is a technique used for analysing categorical data. It creates a new set of variables (called dimensions) that can be used to visualise the relationships between the different categories and to identify patterns and trends.
  
The goal of Non-Negative Matrix Factorization (NMF) is to factorise a given data matrix into two non-negative matrices such that their product approximates the original data matrix. The NMF score measures the squared Euclidean distance between the original data matrix and the reconstructed data matrix obtained. This score refers to the reconstruction error score, which is a measure of how well the original data matrix can be approximated by the low-rank factorization obtained by NMF. The NMF score is useful for determining the appropriate NUMBER of components to use in the factorisation. 

A common approach is to try different values of n_components and choose the value that produces the lowest reconstruction error. However, it is important to note that choosing the value based solely on the reconstruction error may not always lead to the best performance for downstream tasks.
    
   **4.2.2: Selecting optimal n-component score (Categorical)**

  Using the NMF dimension reduction method we may also set out to find whether the Key Insight generated from "Part 2": The RST flag count for all the different types of Malware attacks including Benign can be omitted in the machine learning model stage as it is always not triggered irregardless of the ['Label'].
  
The same NMF Score for all 3 types of Malware attack proves the key insight found in *"Part 2.2: Insights of Exploratory Data Analysis (Categorical)"*, where the RST Flag Count is noted to be the same throughout. This allows us to carry on with the now proven hypothesis that ['RST Flag Count'] can be omitted in the machine learning model stage as it is always not triggered irregardless of the ['Label'].

  This further reduces the actual columns of dimension for the categorical to only 5 Flag Counts therefore although the result acquired from the NMF Dimension Reduction is decent ranging from around 20 - 30 NMF Score at n_components = 4. A lower NMF score indicates a better approximation, while a higher NMF score indicates a worse approximation. We proceeded to continue without Dimension Reduction for the categorical data of the Flag Counts, given that we only have 5 columns of data left and the omission of one more flag count (to make the n_component = 4) is not worth the approximation error reflected in the NMF Score, while any higher would further increased the approximation error.







# 5. [Core Analysis - Machine Learning](../SC1015-mini-project/)
  
  Machine Learning techniques are being used as a quick and efficient means of malware detection. Moving on, we will be applying random forest as our classification model to all the data frames prepared. Our rationale for the chosen model is because of its capability to handle high-dimensional data, as well as our categorical and continuous (numerical) variables. Moreover, it is less prone to overfitting compared to other decision tree-based algorithms. The issue arises when adjusting the class ratio (more samples in the minority class may cause the model to memorise instead of learning generalisable patterns). Our solution is to use cross-validation to counteract this instance of overfitting- increasing the number of folds splits the data into more subsets, allowing the model to be trained and tested on more variations. While this means that the accuracy score obtained from cross-validation can be more reliable, it can also be more computationally expensive to perform.

**5.1 Functions for Resampling and Random Forest Sampling**

Here are the 3 functions we used:

1. def resample_majority (df_minority, df_majority, ratio)
_a function for resampling the majority class in an imbalanced dataset to balance it with the minority class._

2. 
def plot_tree_feature (df, name, n)
_trains a random forest classifier on a given dataframe, and plots the top n feature importances for the model. _

3. def plot_tree (df, name) 
_trains a random forest classifier on a given dataframe, and prints out accuracy scores on both the training and testing sets_
It used as a classification model for dimensionally-reduced (using PCA) dataframes as we are unable to differentiate the resulting components from the dimension reduction process.

**4.2 Model Classification(RandomForestClassifier) on PCA Dimension reduced Numerical DataFrames**

For this section, we are able to utilise these functions to deduce the best ratio in terms of efficiency for reducing computational power required whilst maintaining the accuracy due to the Dimension Reduction process in “Part 3”.

We do acknowledge that the reduction process based on data for “Forward Packets, Backward Packets and Flow” produces relatively the same accuracy for the models across all the different data frames (given the ratio of data distribution).

Thus while dimension reduction can be useful, due to the nature of our dataset, we use it to aid us to find the right ratio for data distribution in terms of efficiency and accuracy. 

We will be utilising the "general reduced control" variables as the set to determine which ratio for the the distribution of [Types of Malware Attacks : Benign] to proceed on for the Numerical Data. This is because the PCA score for n_components = 9 is > 96, which results in 9 principle components instead of the 14 from the default Numerical dataset. This helps us make the machine learning process less computationally expensive, improving efficiency whilst retaining high accuracy.

From this RandomForestClassifier, we deduce that 90:10 ratio of data distribution [Malware Attack: Benign] offers the best prediction accuracy, and we will thus be using this ratio. On this page, we can view the accuracy scores generated for each type of malware, for the respective categories of numerical variables. The volume of "Positive Malware Attacks" will aid us in better predicting the variables that correlates to each type of positive malware attack.

**4.3 Model Classification(RandomForestClassifier) on Numerical DataFrames**

Based on the information provided, we can conclude that the Random Forest Classifier model was able to accurately predict the presence of malware with an accuracy score of around 90%. The order of feature importance suggests that these variables in red and orange on this slide are the most important numerical features for predicting the presence of malware. These insights allow us to create efficient methods for identifying and thwarting malware assaults.

For instance, security experts can concentrate on keeping an eye on the network traffic and data Flow Data in Android smartphones to spot suspicious behaviour connected to these crucial elements. To increase the accuracy of malware detection on Android devices, they can also create machine learning models like this basic model we created in predicting which variable. 

**4.4 Model Classification(RandomForestClassifier) on Categorical DataFrames for the Flag Counts**

Comparing across the types of malware, we can see that the count of SYN flags is consistently the most important feature. The order of importance for other flags varies - this is useful in developing effective strategies for detecting different types of Android malware. For example, security professionals can focus on monitoring the network traffic for the count of SYN flags to detect potential Adware, Scareware, and SMS malware attacks.



# 6. [Conclusion](../SC1015-mini-project/)

Based on the analysis of numerical data and categorical data, we are able to achieve an accuracy of >90% in predicting the variables as indicators for the various types of malware attacks. We can now answer the original question: What are the variables that help to predict the types of malware attack that happened on an Android device? Overall we can conclude that:

From the numerical data, we can realise which are the important features for predicting the presence of malware, regardless of the type of malware. 

Coupled with the insights from the categorical data, we can see that the order of importance for these flags varies, which can help predict which type of Malware Attack has occurred on the Android Device.

Leveraging on these findings, they may aid us if we were to now set out to create a machine learning model for Android device malware prediction, allowing for more precise and efficient mitigation of Android Malware Attacks.

