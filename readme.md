# CLASSIFICATION PROJECT

### A Model to Help Prospective Patients Estimate the Likelihood of Mortality Following an Elective Surgery

## Abstract

The Cleveland Clinic performs thousands elective surgical procedures each year, and unfortunately a small percentage of them (roughly 0.40%) result in patient mortality within 30 days after surgery. As these are elective procedures, the patient has the right to make an informed choice based on perceived risks and may ultimately decide to avoid surgery.

To assist with such decisions, the clinic would like to provide prospective elective surgery patients more than just the average mortality percentage cited above. Clearly, based on individual health factors, diagnoses and specific procedures, individual risk of mortality can vary significantly from patient to patient. Thus, the clinic would like to have a model that can provide individualized predictions of the likelihood of mortality.

## Design

Predictive models were developed using past records of general elective surgery patient encounters at the Cleveland Clinic spanning a 5-year and 9-month period. Each encounter had already been de-identified to protect patient privacy, and the data was divided up into a held-out test set (20% of the records), a training set (80% of the remainder), and a validation set (20% of the remainder). Various models were developed and assessed using 5-fold cross-validation on the training set and then comparing AUROC (Area Under the Receiver Operating Curve) results of each model between the training and validation sets. 

Binary variables in the dataset were one-hot encoded, with one column dropped for Logistic Regression models and no columns dropped for tree-based classification models. Moreover, although medical domain knowledge was not readily available to help with informed feature engineering, non-binary variables were expanded into additional features using common transformations: log (where feasible), exponential, squares, and cubed values.

After all models had been developed and tuned, each was retrained on the training+validation sets and evaluted for final performance based on how well it performed on the held-out test set. The performace goals were a Recall (True Positive Rate) of at least 0.95 with a False Positive Rate as small as possible (but no greater than 0.10). The model which most exeeded these performance parameters was designated as the final deliverable model.

## Data

The dataset provided by Cleveland Clinic for modeling purposes was originally compiled for and used in a study looking into whether or not surgical timing (day of week, hour of day, time of year) has a significant effect on surgical outcomes (see the published abstract here: https://pubmed.ncbi.nlm.nih.gov/21965365/). 

Each sample (patient encounter) from the dataset consisted of the following 23 predictors: 

| Predictor                                                    | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Procedure Category (string)                                  | Standardized category of surgical procedure performed        |
| Age (float)                                                  | Patient age in years at the time of surgery                  |
| Gender (binary)                                              | Patient gender                                               |
| Race (integer)                                               | Patient race (categorized as Caucasian, African American or Other) |
| ASA Status (integer)                                         | Standardized patient anathesiological status indicator (takes on values of 1, 2 or 3) |
| BMI (float)                                                  | Body Mass Index (in kg/meters-squared)                       |
| Baseline_Cancer (binary)                                     | 1= has Cancer, 0 = does not                                  |
| Baseline_CVD (binary)                                        | 1 = has Cardivascular/Cerebrovascular Disease, 0 = does not  |
| Baseline_Dementia (binary)                                   | 1 = has Dementia, 0 = does not                               |
| Baseline_Diabetes (binary)                                   | 1 = has Diabetes, 0 = does not                               |
| Baseline_Digestive (binary)                                  | 1 = has Digestive Disease, 0 = does not                      |
| Baseline_Osteoart (binary)                                   | 1 = has Osteoarthritis, 0 = does not                         |
| Baseline_Psych (binary)                                      | 1 = has Psychiatric Disorder, 0 = does not                   |
| Baseline_Pulmonary (binary)                                  | 1 = has Pulmonary Disease, 0 = does not                      |
| Charlson Comorbitity Index (integer)                         | > = 0; grows with number of simultaneous conditions/diagnoses/risk factors (including age) |
| Risk Stratification Index: 30-Day Mortality (float)          | Standardized system for comparing 30-day post-surgery mortality outcomes across medical institutions |
| Risk Stratification Index: In-Hospital Complications (float) | Standardized system for comparing in-hospital complication outcomes across medical institutions |
| ccsMort30Rate (float)                                        | Overall Incidence of 30-Day Mortality (rate) for each Procedure Category (these are standardized values) |
| ccsComplicationRate (float)                                  | Overall Incidence of In-Hospital Complications (rate) for each Procedure Category (these are standardized values) |
| Hour (float)                                                 | Hour of the day when surgery began                           |
| DOW (integer)                                                | Day of the week when surgery was performed                   |
| Month (integer)                                              | Month when surgery was performed                             |
| Moonphase (integer)                                          | Phase (quarter) of the moon when surgery was performed       |



The dataset had two target variables:

| Target                            | Description                               |
| --------------------------------- | ----------------------------------------- |
| 30-Day Mortality (binary)         | 1 = Patient died, 0 = did not             |
| In-Hospital Complication (binary) | 1 = Complication(s) occurred, 0 = did not |



The target of interest, 30-Day Mortality, was highly-imbalanced: 99.586% zeroes and only 0.414% ones.

(Note that a secondary objective of this project was to also predict the other target, In-Hospital Complication. There did not end up being enough time to thoroughly pursue this objective, but based on early data exploration and preliminary modeling, it appeared to be a more difficult outcome to predict. This may have to do with the inherently random nature of in-hospital complications, whereas mortality may be more clearly indicated in positive cases based on the patient variables contained in the dataset.)

## Algorithms

The primary classification models developed for this project were: 

- Logistic Regressions, using scaled data and elasticnet regularization (tuned on both AUC and separately on log-loss as optimization metrics, resulting in two different models)
- Random Forest Classifiers, using the unscaled dataset (and hyperparameter tuned on both AUC and log-loss as optimization metrics, resulting in two distinct models)
- XGBoost Classifiers, using the unscaled dataset (and hyperparameter tuned on both AUC and log-loss optimization metrics, resulting in two separate models)
- "Soft" Voting Classifier ensembles, using several combinations of the models listed above

For each non-ensembled model, both unbalanced data and various data balancing techniques (class weighting, Random Oversampling, and SMOTE) were tried to obtain the best possible model performance.

The models were all designed to provide soft classification decisions (continuous values between 0.0 and 1.0) so that once an optimal hard decision threshold was chosen (to distinguish between "No Elevated Risk of Mortality" and "Elevated Risk of Mortality" classifications), the "Elevated Risk of Mortality" category could be further broken down into relative elevated risk strata (e.g., "Low Elevated Risk", "Medium Elevated Risk", "High Elevated Risk") based on the ranked values of the soft decision predictions.

## Tools 

The following tools were used in this project:

1. Pandas to clean, explore, feature engineer and generate the final modeling data
2. SKLearn to implement various soft decision classification models as well as to perform data balancing, cross validation, and regularization
3. Matplotlib and Seaborn to visualize the data and model outputs, and to present final results
4. Python 3.8 (to be able to use Pandas, SKLearn, Matplotlib and Seaborn)

## Communication

In addtition to presenting final Project Slides to the stakeholders, all work (including the slides) can be found here: https://github.com/georgepappy/Classification

