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
6. Data Driven Insights and Conclusion
7. References

# 1. [Problem Formulation](../SC1015-mini-project/)

## Background
The spread of malware is a growing issue with severe consequences. According to Kaspersky Security Network, in Q3 2022, a total of 5,623,670 mobile malware, adware, and riskware attacks were blocked. Thus, in order to investigate our cybersecurity, we decided to analyse the capabilities of android malware detection models.

## Motivation
When it comes to preventing malware attacks, there are multiple different vantage points in which we can tackle the issue from. For this project, we have chosen to focus specifically on leveraging network data to detect malware. Why did we choose this approach? Well, malware often communicates with command-and-control servers over the network, and this communication can be captured in network flow data. We discovered that by analyzing network flow data and detecting patterns, we are able to identify the suspiciousness of an application, which aids in malware detection.

The objective of our project is two-fold. Firstly, we aim to achieve good accuracy in detecting malware from samples of benign and malware applications using 2 approaches. We began by selecting features to analyse based on their definitions, and proceeded to use dimension reduction to further improve this analysis. Dimension reduction aids with simplifying and optimising data analysis and machine learning algorithms. Secondly, we compared features/characteristics of different data types to provide recommendations on the best strategy for malware detection. This allows us to resolve the model selection problem: weighing between logistic regression (categorical) vs random forest (numerical).

## Our question: 
## Our dataset: Android Malware Detection on Kaggle (updated-version Feb 2023)
The database for this project was extracted from Kaggle. It has 355630 Data points and it contains features of network flow characteristics and statistics. *click* The data is split into four categories, software labelled ‘Android adware’, ‘Android Scareware’, ‘Android SMS Malware’ and ‘Benign’. These will be the response variables we will be examining, where the remaining variables in our dataset will act as listed predictors that we will be using in this study.*click*

# 2. [Data Preparation and Cleaning](../SC1015-mini-project/)
  In this section of the project, we prepared and cleaned the kaggle dataset. We split the dataset into 4 other types and 
  (1)EDA and (2)Dimension reduction in the later sections.

  We performed the following:

  ##1. Preliminary Feature Selection: 21 relevant variables out of 85 were selected, which can be categorised into 4 types:
    (1) Total Length of Fwd Packets and Total Length of Bwd Packets: These features represent the total length of data transmitted in the forward and backward directions during a flow. Similar to the total packet counts, anomalously high volumes of data transmission can be indicative of malware activity.
    (2) Fwd Packet Length Max, Fwd Packet Length Mean, Fwd Packet Length Std, Bwd Packet Length Max, Bwd Packet Length Mean, and Bwd Packet Length Std: 
These features represent various statistics of packet length for both the forward and backward directions during a flow. Malware may use specific packet length patterns to hide its communication, which can be identified through these statistics. For example, high standard deviations in packet length can be an indicator of encrypted traffic, which may be used by malware to hide its communication.
    (3) Flow Bytes/s and Flow Packets/s: These features represent the rate of bytes and packets transmitted per second during a flow. High rates can indicate potential malware activity.
    (4) FIN Flag Count, SYN Flag Count, RST Flag Count, PSH Flag Count, ACK Flag Count, and URG Flag Count: These features represent the count of different TCP flags set during a flow. Some types of malware may use specific TCP flag combinations for their communication.
  
  ##2. Splitting the dataset in the different type of possible malware data
      For the purpose of our analysis, we split our dataset depending on the type of malware attacks for exploratory analysis: 4 DataFrames containing variables relating to: Android_Adware attacks, Android_Scareware attacks, Android_SMS_Malware attacks, with Benign attacks as control. Further data cleaning and preparation are done separately for all dataFrames.
  ##3. Encoding categorical data (label)
      The categorical variable that we are trying to predict is “Label” - which consists of the 4 different types of android attacks. In order to do any form of analysis on this, we must encode it. For this, we use LabelEncoder from the sklearn.preprocessing library.
  
  ##4. Conversion of final cleaned and prepared dataframes to pickle file
 
#3. [Exploratory Data Analysis and Visualisation: Training the models and Predicting Test Data](../SC1015-mini-project/)
  ##3.1.1: Exploratory Data Analysis (Numerical)
  
  ##3.1.2: Insights of Exploratory Data Analysis (Numerical)
  ##3.2.1: Exploratory Data Analysis (Categorical)
  ##3.2.2: Insights of Exploratory Data Analysis (Categorical)


