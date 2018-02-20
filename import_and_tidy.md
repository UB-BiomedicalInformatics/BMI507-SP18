Kaggle Data Import & Tidy
================

Load Required Packages
----------------------

``` r
library(tidyverse)
```

    ## ── Attaching packages ────────

    ## ✔ ggplot2 2.2.1     ✔ purrr   0.2.4
    ## ✔ tibble  1.3.4     ✔ dplyr   0.7.4
    ## ✔ tidyr   0.7.2     ✔ stringr 1.2.0
    ## ✔ readr   1.1.1     ✔ forcats 0.2.0

    ## ── Conflicts ─────────────────
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

List training files
-------------------

``` r
training_files <- list.files(path = "sample_data/train_data/")
training_files
```

    ##  [1] "training_allergy.csv"             
    ##  [2] "training_allMeds.csv"             
    ##  [3] "training_diagnosis.csv"           
    ##  [4] "training_immunization.csv"        
    ##  [5] "training_labObservation.csv"      
    ##  [6] "training_labPanel.csv"            
    ##  [7] "training_labResult.csv"           
    ##  [8] "training_labs.csv"                
    ##  [9] "training_medication.csv"          
    ## [10] "training_patient.csv"             
    ## [11] "training_patientCondition.csv"    
    ## [12] "training_patientSmokingStatus.csv"
    ## [13] "training_patientTranscript.csv"   
    ## [14] "training_prescription.csv"        
    ## [15] "training_smoke.csv"               
    ## [16] "training_transcript.csv"          
    ## [17] "training_transcriptAllergy.csv"   
    ## [18] "training_transcriptDiagnosis.csv" 
    ## [19] "training_transcriptMedication.csv"

List testing files
------------------

``` r
testing_files <- list.files(path = "sample_data/test_data/")
testing_files
```

    ##  [1] "test_allergy.csv"              "test_allMeds.csv"             
    ##  [3] "test_diagnosis.csv"            "test_immunization.csv"        
    ##  [5] "test_labObservation.csv"       "test_labPanel.csv"            
    ##  [7] "test_labResult.csv"            "test_labs.csv"                
    ##  [9] "test_medication.csv"           "test_patient.csv"             
    ## [11] "test_patientCondition.csv"     "test_patientSmokingStatus.csv"
    ## [13] "test_patientTranscript.csv"    "test_prescription.csv"        
    ## [15] "test_smoke.csv"                "test_transcript.csv"          
    ## [17] "test_transcriptAllergy.csv"    "test_transcriptDiagnosis.csv" 
    ## [19] "test_transcriptMedication.csv"

Confirm file counts
-------------------

``` r
c(length(testing_files), length(training_files))
```

    ## [1] 19 19

Confirm matching files
----------------------

``` r
training_files_names <- str_replace(training_files, "training_", "")
testing_files_names <- str_replace(testing_files, "test_", "")
table(training_files_names, testing_files_names)
```

    ##                           testing_files_names
    ## training_files_names       allergy.csv allMeds.csv diagnosis.csv
    ##   allergy.csv                        1           0             0
    ##   allMeds.csv                        0           1             0
    ##   diagnosis.csv                      0           0             1
    ##   immunization.csv                   0           0             0
    ##   labObservation.csv                 0           0             0
    ##   labPanel.csv                       0           0             0
    ##   labResult.csv                      0           0             0
    ##   labs.csv                           0           0             0
    ##   medication.csv                     0           0             0
    ##   patient.csv                        0           0             0
    ##   patientCondition.csv               0           0             0
    ##   patientSmokingStatus.csv           0           0             0
    ##   patientTranscript.csv              0           0             0
    ##   prescription.csv                   0           0             0
    ##   smoke.csv                          0           0             0
    ##   transcript.csv                     0           0             0
    ##   transcriptAllergy.csv              0           0             0
    ##   transcriptDiagnosis.csv            0           0             0
    ##   transcriptMedication.csv           0           0             0
    ##                           testing_files_names
    ## training_files_names       immunization.csv labObservation.csv
    ##   allergy.csv                             0                  0
    ##   allMeds.csv                             0                  0
    ##   diagnosis.csv                           0                  0
    ##   immunization.csv                        1                  0
    ##   labObservation.csv                      0                  1
    ##   labPanel.csv                            0                  0
    ##   labResult.csv                           0                  0
    ##   labs.csv                                0                  0
    ##   medication.csv                          0                  0
    ##   patient.csv                             0                  0
    ##   patientCondition.csv                    0                  0
    ##   patientSmokingStatus.csv                0                  0
    ##   patientTranscript.csv                   0                  0
    ##   prescription.csv                        0                  0
    ##   smoke.csv                               0                  0
    ##   transcript.csv                          0                  0
    ##   transcriptAllergy.csv                   0                  0
    ##   transcriptDiagnosis.csv                 0                  0
    ##   transcriptMedication.csv                0                  0
    ##                           testing_files_names
    ## training_files_names       labPanel.csv labResult.csv labs.csv
    ##   allergy.csv                         0             0        0
    ##   allMeds.csv                         0             0        0
    ##   diagnosis.csv                       0             0        0
    ##   immunization.csv                    0             0        0
    ##   labObservation.csv                  0             0        0
    ##   labPanel.csv                        1             0        0
    ##   labResult.csv                       0             1        0
    ##   labs.csv                            0             0        1
    ##   medication.csv                      0             0        0
    ##   patient.csv                         0             0        0
    ##   patientCondition.csv                0             0        0
    ##   patientSmokingStatus.csv            0             0        0
    ##   patientTranscript.csv               0             0        0
    ##   prescription.csv                    0             0        0
    ##   smoke.csv                           0             0        0
    ##   transcript.csv                      0             0        0
    ##   transcriptAllergy.csv               0             0        0
    ##   transcriptDiagnosis.csv             0             0        0
    ##   transcriptMedication.csv            0             0        0
    ##                           testing_files_names
    ## training_files_names       medication.csv patient.csv patientCondition.csv
    ##   allergy.csv                           0           0                    0
    ##   allMeds.csv                           0           0                    0
    ##   diagnosis.csv                         0           0                    0
    ##   immunization.csv                      0           0                    0
    ##   labObservation.csv                    0           0                    0
    ##   labPanel.csv                          0           0                    0
    ##   labResult.csv                         0           0                    0
    ##   labs.csv                              0           0                    0
    ##   medication.csv                        1           0                    0
    ##   patient.csv                           0           1                    0
    ##   patientCondition.csv                  0           0                    1
    ##   patientSmokingStatus.csv              0           0                    0
    ##   patientTranscript.csv                 0           0                    0
    ##   prescription.csv                      0           0                    0
    ##   smoke.csv                             0           0                    0
    ##   transcript.csv                        0           0                    0
    ##   transcriptAllergy.csv                 0           0                    0
    ##   transcriptDiagnosis.csv               0           0                    0
    ##   transcriptMedication.csv              0           0                    0
    ##                           testing_files_names
    ## training_files_names       patientSmokingStatus.csv patientTranscript.csv
    ##   allergy.csv                                     0                     0
    ##   allMeds.csv                                     0                     0
    ##   diagnosis.csv                                   0                     0
    ##   immunization.csv                                0                     0
    ##   labObservation.csv                              0                     0
    ##   labPanel.csv                                    0                     0
    ##   labResult.csv                                   0                     0
    ##   labs.csv                                        0                     0
    ##   medication.csv                                  0                     0
    ##   patient.csv                                     0                     0
    ##   patientCondition.csv                            0                     0
    ##   patientSmokingStatus.csv                        1                     0
    ##   patientTranscript.csv                           0                     1
    ##   prescription.csv                                0                     0
    ##   smoke.csv                                       0                     0
    ##   transcript.csv                                  0                     0
    ##   transcriptAllergy.csv                           0                     0
    ##   transcriptDiagnosis.csv                         0                     0
    ##   transcriptMedication.csv                        0                     0
    ##                           testing_files_names
    ## training_files_names       prescription.csv smoke.csv transcript.csv
    ##   allergy.csv                             0         0              0
    ##   allMeds.csv                             0         0              0
    ##   diagnosis.csv                           0         0              0
    ##   immunization.csv                        0         0              0
    ##   labObservation.csv                      0         0              0
    ##   labPanel.csv                            0         0              0
    ##   labResult.csv                           0         0              0
    ##   labs.csv                                0         0              0
    ##   medication.csv                          0         0              0
    ##   patient.csv                             0         0              0
    ##   patientCondition.csv                    0         0              0
    ##   patientSmokingStatus.csv                0         0              0
    ##   patientTranscript.csv                   0         0              0
    ##   prescription.csv                        1         0              0
    ##   smoke.csv                               0         1              0
    ##   transcript.csv                          0         0              1
    ##   transcriptAllergy.csv                   0         0              0
    ##   transcriptDiagnosis.csv                 0         0              0
    ##   transcriptMedication.csv                0         0              0
    ##                           testing_files_names
    ## training_files_names       transcriptAllergy.csv transcriptDiagnosis.csv
    ##   allergy.csv                                  0                       0
    ##   allMeds.csv                                  0                       0
    ##   diagnosis.csv                                0                       0
    ##   immunization.csv                             0                       0
    ##   labObservation.csv                           0                       0
    ##   labPanel.csv                                 0                       0
    ##   labResult.csv                                0                       0
    ##   labs.csv                                     0                       0
    ##   medication.csv                               0                       0
    ##   patient.csv                                  0                       0
    ##   patientCondition.csv                         0                       0
    ##   patientSmokingStatus.csv                     0                       0
    ##   patientTranscript.csv                        0                       0
    ##   prescription.csv                             0                       0
    ##   smoke.csv                                    0                       0
    ##   transcript.csv                               0                       0
    ##   transcriptAllergy.csv                        1                       0
    ##   transcriptDiagnosis.csv                      0                       1
    ##   transcriptMedication.csv                     0                       0
    ##                           testing_files_names
    ## training_files_names       transcriptMedication.csv
    ##   allergy.csv                                     0
    ##   allMeds.csv                                     0
    ##   diagnosis.csv                                   0
    ##   immunization.csv                                0
    ##   labObservation.csv                              0
    ##   labPanel.csv                                    0
    ##   labResult.csv                                   0
    ##   labs.csv                                        0
    ##   medication.csv                                  0
    ##   patient.csv                                     0
    ##   patientCondition.csv                            0
    ##   patientSmokingStatus.csv                        0
    ##   patientTranscript.csv                           0
    ##   prescription.csv                                0
    ##   smoke.csv                                       0
    ##   transcript.csv                                  0
    ##   transcriptAllergy.csv                           0
    ##   transcriptDiagnosis.csv                         0
    ##   transcriptMedication.csv                        1
