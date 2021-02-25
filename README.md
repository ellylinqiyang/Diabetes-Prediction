# UChicago MSCA 32009 Health Analytics Project
[Kaggle Competition](https://www.kaggle.com/c/pf2012-diabetes/): Practice Fusion Diabetes Classification

## Description
Over 25 million people, or nearly 8.3% of the entire United States population, have diabetes. Diabetes is also associated with a wide range of complications from heart disease and stroke to blindness and kidney disease. Predicting who has diabetes will lead to a better understanding of these complications and the common comorbidities that diabetics suffer.

The Challenge: Given a de-identified data set of patient electronic health records, build a model to determine who has a diabetes diagnosis, as defined by ICD9 codes 250, 250.0, 250.*0 or 250.*2 (e.g., 250, 250.0, 250.00, 250.10, 250.52, etc).

## Dataset Modification for Competition
Although the files names are the same as those released in the Prospect Phase of the Prediction Challenge, the files available here have been modified in order to facilitate the diabetes classification competition.

A number of details from the data sets need to be removed in order to not give the answer away. These features are removed from both the training as well as test set so that models do not reference stripped variables as features. Added to training_SyncPatient is an indicator column designating who has a diagnosis of T2DM as defined by ICD9 codes 250, 250.0, 250.*0 or 250.*2. This is the quantity to be predicted by the competition.

Data removal is restricted to the particular item, not the entire record or visit or entirety of lab results. For example, if a metabolic panel is ordered, then only the glucose-related results are removed while the other results remain.

Here's a brief outline of the steps taken to prepare the original dataset into the training set and test sets. Note that the test data was NOT released as part of the original data release.

Remove patients that have diagnoses for diabetes complications without a basic diagnosis of Type II diabetes mellitus (T2DM), as defined by ICD9 codes 250, 250.0, 250.*0 or 250.*2, from all tables in both data sets. This is 46 patients in the training set, 20 patients in test set. Removing patients requires removing all instances of their PatientGuid from ALL TABLES.

Remove rows from SyncDiagnosis with ICD9 codes 250, 250.0, 250.*0 or 250.*2 or ICD9 codes for diabetes complications 357.2, 362.0* (e.g., 362.01, 362.05), or 250.1, 250.2, ... , 250.9. These rows identify a set of DiagnosisGuid that need to be removed from SyncTranscriptDiagnosis. Remove the field 'OrderBy' in SyncTranscriptDiagnosis entirely.
Remove rows from SyncMedication which references a diabetes medication. These rows identify a set of MedicationGuid that need to be removed from SyncPrescription and SyncTranscriptMedication. Remove the field 'OrderBy' in SyncTranscriptMedication entirely.

Remove rows from LabObservation where the HL7Text identifies glucose or insulin related labs. These rows identify a set of LabObservationGuid that need to be removed from LabResult. The rows in LabResult identify a set of LabResultGuid that need to be removed from LabResult and LabPanel. Remove any rows from LabPanel where the panel name relates to glucose, insulin HbA1c, etc measurements.