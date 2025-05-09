# Optimizing an ML Pipeline in Azure

## Overview
This project is part of the Udacity Azure ML Nanodegree.
In this project, we build and optimize an Azure ML pipeline using the Python SDK and a provided Scikit-learn model.
This model is then compared to an Azure AutoML run.

![image](https://github.com/user-attachments/assets/cd05b91f-e40c-490a-bd1a-6264dbf6c2e7)


## Useful Resources
- [ScriptRunConfig Class](https://docs.microsoft.com/en-us/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py)
- [Configure and submit training runs](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-set-up-training-targets)
- [HyperDriveConfig Class](https://docs.microsoft.com/en-us/python/api/azureml-train-core/azureml.train.hyperdrive.hyperdriveconfig?view=azure-ml-py)
- [How to tune hyperparamters](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-tune-hyperparameters)


## Summary
**In 1-2 sentences, explain the problem statement: e.g "This dataset contains data about... we seek to predict..."**

The dataset originates from a marketing campaign and includes details about individuals who were contacted over the phone. The objective is to forecast whether a person will respond positively ("yes") and become a lead, the yes is represented in the column "y" which is the target column for this exercise.
Tabel overview:

![image](https://github.com/user-attachments/assets/d63990bc-0675-47db-99d5-04111828bc43)



It should be highlighted that the data contains large amont of "no" in column "y", as seen when exploring the data with azure.

![image](https://github.com/user-attachments/assets/05a10897-bd3b-45df-8318-f12dd6ed76c5)


**In 1-2 sentences, explain the solution: e.g. "The best performing model was a ..."**

The best performing model based on accuracy was the automl model, which performed 0.9166920.
While the hyperdrive performed 0.9141782.

### AutoML (Project1automl)
![image](https://github.com/user-attachments/assets/a2405b2a-4503-4642-b756-5a6c135ac141)


### Hyperdrive (HD)
![image](https://github.com/user-attachments/assets/82952ad7-65db-4ad0-a190-bd2111e94798)



## Scikit-learn Pipeline
**Explain the pipeline architecture, including data, hyperparameter tuning, and classification algorithm.**

To begin, the tabular data was imported and prepared by removing any inconsistencies or irrelevant information. fx, cleaning of the target column, changing yes and no into 1 and 0.

y_df = x_df.pop("y").apply(lambda s: 1 if s == "yes" else 0)

Afterwards we add two parameters for logistic regression: 

Inverse regularization strength:
Which was set to either 0.1, 0.5, 1, or 5, with 1 being the default setting. 

Maximum number of iterations:
Which was set to either 50, 100, or 500, with the default being 100. 

Both parameters supporting the classification task for the  logistic regression, utilizing them to make predictions.

**What are the benefits of the parameter sampler you chose?**

As I understand, the main benefit is the parameters has the ability to handle hyperparameters efficiently, without overwhelming/overclocking the compute ressoruces thus not limiting the performance.
As well as being rather easy to fine tune.

**What are the benefits of the early stopping policy you chose?**

In my assignment, I used Bandit policy, as it manages resources very efficiently by terminating jobs that do not perform well compared to the best job, using a defined slack factor.

policy = BanditPolicy(evaluation_interval=2, slack_factor=0.1)

## AutoML
**In 1-2 sentences, describe the model and hyperparameters generated by AutoML.**

The best automl model was voting ensemble with 3 hyperparameters:

Light GMB occuring 3 times with an accumulated weight of 50%

LogisticRegression occruing 4 times with an accumulated weight of 36%

Random forrest occuring 1 time with a score of 14%

(See images under appendix at the end of scope pr. hyper parameter)

![image](https://github.com/user-attachments/assets/178645c7-e64d-435d-ba93-a3d23c1e45aa)


## Pipeline comparison
**Compare the two models and their performance. What are the differences in accuracy? In architecture? If there was a difference, why do you think there was one?**

The Automl performed with an accuracy on 0.9166920 and the hyperdrive performed 0.9141782.
The difference between the two models accuracy aint much and they have access to the same date and quality process.
The autoML's performance could be related to the access of many different models and more automated approach.

## Future work
**What are some areas of improvement for future experiments? Why might these improvements help the model?**

1. First recommnedation would be data quality, as mentioned in the beginning there is a lot of "no" in the analyzed column, so either that has to be taken into consideration in the code or potentially just gathering more data.

2. Change the timeout or termination policy - By changing the time-out or termination policy for the automl, it could potentially have found a better model - however, this result in more ressources etc. (incl. patience)

## Proof of cluster clean up
**If you did not delete your compute cluster in the code, please complete this section. Otherwise, delete this section.**
**Image of cluster marked for deletion**
Done in the code:

![image](https://github.com/user-attachments/assets/ed7e4b74-8776-4acd-8962-a0d41211013c)

Cluster page empty after delection

![image](https://github.com/user-attachments/assets/51701757-8177-42f0-8ff5-3277af5b57c2)

## Appendix - Scores
![image](https://github.com/user-attachments/assets/1b1af3cf-44a0-4fae-8b0f-878d15124c5e)
![image](https://github.com/user-attachments/assets/4dca526a-ecd7-430f-b024-499acdaf7664)
![image](https://github.com/user-attachments/assets/e9cd4389-42a8-4758-afe4-3db75e17838c)
![image](https://github.com/user-attachments/assets/f6aa901d-c0f8-4697-bc5d-3e548890cf0f)
![image](https://github.com/user-attachments/assets/887d099f-26a9-4b83-aaf6-02c3c016ca0c)
![image](https://github.com/user-attachments/assets/70e7516c-20ac-4246-ab2b-fbb8a720f74b)
![image](https://github.com/user-attachments/assets/731f767e-ac49-4b9b-be82-27c2d88ece61)
![image](https://github.com/user-attachments/assets/d7aa2201-4445-44fd-b468-d2ebaa7fbb9d)








