# PROJECT PROPOSAL

##### CAN A PREDICTIVE MODEL BE BUILT TO GIVE PROSPECTIVE PATIENTS A REASONABLE ESTIMATE OF THE RELATIVE LIKELIHOOD OF MORTALITY SHOULD THEY CHOOSE TO PROCEED WITH AN ELECTIVE SURGERY? CAN A SIMILAR MODEL BE DEVELOPED TO PREDICT THE RELATIVE LIKELIHOOD OF SURGICAL COMPLICATIONS? 

The Cleveland Clinic performs many elective surgical procedures each year, and unfortunately a small percentage of them (roughly 0.40%) result in patient mortality within 30 days after surgery. Many more of such surgeries (approximately 13.2%) result in some sort of complication during the associated hospital stay. As these are elective procedures, the patient has the right to make an informed choice based on these relative risks and may ultimately decide to avoid surgery.

To assist with such decisions, the clinic would like to provide prospective elective surgery patients more than just the average percentages for the risk of mortality and in-hospital complications cited above. Clearly, based on individual health factors, diagnoses and specific procedures, those risks can vary significantly from patient to patient. Thus, the clinic would like to have models that can provide individualized predictions of the relative likelihood of mortality and complications.

The raw data to be used in building these predictive models consists of 32,001 general elective surgery patient encounters at the Cleveland Clinic spanning a 5-year and 9-month period. Each encounter has been de-identified to protect patient privacy, and the entire dataset has been compiled for and used in a study looking into whether or not surgical timing (day of week, hour of day, time of year) has a significant effect on surgical outcomes (see the published abstract here: https://pubmed.ncbi.nlm.nih.gov/21965365/). 

Each sample (patient encounter) from the dataset consists of the following 23 predictors: 

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



The dataset has two target variables of interest:

| Target                            | Description                               |
| --------------------------------- | ----------------------------------------- |
| 30-Day Mortality (binary)         | 1 = Patient died, 0 = did not             |
| In-Hospital Complication (binary) | 1 = Complication(s) occurred, 0 = did not |



As previously indicated, this project will predict the relative likelihoods of a positive outcome for both of the targets above based on some combination of the predictors discussed (and any additional features engineered from them). The baseline models will be Logistic Regressions using features deemed to be most meaningful for the task based on preliminary exploratory data analysis. From there, variable selection and regularization will be applied in an effort to find the model that performs best. Moreover, tree-based techniques (e.g., Random Forrest, XGBoost) will also be employed in an attempt to improve performance.

Since both targets are imbalanced (severely in the case of Mortality), various mitigation techniques (including sampling, class weighting and threshold adjustment) will be employed in an attempt to improve and maximize the performance of these models.

The tools required for this project are: 

1. Pandas to clean, explore and generate the final modeling data
3. SKLearn to implement various soft decision classification models as well as to perform data balancing, cross validation, variable selection and regularization
4. Matplotlib and Seaborn to visualize the data and present final results
5. Python 3.8 (to be able to use Pandas, SKLearn, Matplotlib and Seaborn)

The Minimum Viable Product for this project will be a report (and slides) presenting models optimally tuned to predict the relative likelihood of 30-Day Mortality and In-Hospital Complication for a given prospective patient based on the best subset of preditors and engineered features. Model quality will be demonstrated with graphs (Receiver Operating Characteristic Curves, Precision/Recall Curves, Threshold vs. F1/Precision/Recall) and by citing figures of merit such as AUROC, Precision, Recall and F1 Score.

