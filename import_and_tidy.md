Kaggle Data Import & Tidy
================

Load Required Packages
----------------------

``` r
library(tidyverse)
```

    ## ── Attaching packages ──────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 2.2.1     ✔ purrr   0.2.4
    ## ✔ tibble  1.3.4     ✔ dplyr   0.7.4
    ## ✔ tidyr   0.7.2     ✔ stringr 1.2.0
    ## ✔ readr   1.1.1     ✔ forcats 0.2.0

    ## ── Conflicts ─────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(stringr)
```

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

``` r
training_data <- training_files %>% 
    map(~paste0("sample_data/train_data/", .x)) %>% 
    map(read_csv)
```

    ## Parsed with column specification:
    ## cols(
    ##   AllergyGuid = col_character(),
    ##   PatientGuid = col_character(),
    ##   AllergyType = col_character(),
    ##   StartYear = col_integer(),
    ##   ReactionName = col_character(),
    ##   SeverityName = col_character(),
    ##   MedicationNdcCode = col_double(),
    ##   MedicationName = col_character(),
    ##   UserGuid = col_character()
    ## )

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_character(),
    ##   PrescriptionYear = col_integer(),
    ##   RefillAsNeeded = col_integer(),
    ##   GenericAllowed = col_integer(),
    ##   StartYear = col_integer(),
    ##   `MedicationNdcCode:1` = col_double()
    ## )

    ## See spec(...) for full column specifications.

    ## Parsed with column specification:
    ## cols(
    ##   DiagnosisGuid = col_character(),
    ##   PatientGuid = col_character(),
    ##   ICD9Code = col_character(),
    ##   DiagnosisDescription = col_character(),
    ##   StartYear = col_integer(),
    ##   StopYear = col_character(),
    ##   Acute = col_integer(),
    ##   UserGuid = col_character()
    ## )

    ## Parsed with column specification:
    ## cols(
    ##   ImmunizationGuid = col_character(),
    ##   PatientGuid = col_character(),
    ##   VaccineName = col_character(),
    ##   AdministeredYear = col_integer(),
    ##   CvxCode = col_integer(),
    ##   UserGuid = col_character()
    ## )

    ## Parsed with column specification:
    ## cols(
    ##   HL7Identifier = col_character(),
    ##   HL7Text = col_character(),
    ##   LabObservationGuid = col_character(),
    ##   LabPanelGuid = col_character(),
    ##   HL7CodingSystem = col_character(),
    ##   ObservationValue = col_double(),
    ##   Units = col_character(),
    ##   ReferenceRange = col_character(),
    ##   AbnormalFlags = col_character(),
    ##   ResultStatus = col_character(),
    ##   ObservationYear = col_integer(),
    ##   UserGuid = col_character(),
    ##   IsAbnormalValue = col_integer()
    ## )

    ## Warning in rbind(names(probs), probs_f): number of columns of result is not
    ## a multiple of vector length (arg 1)

    ## Warning: 4692 parsing failures.
    ## row # A tibble: 5 x 5 col     row              col expected actual expected   <int>            <chr>    <chr>  <chr> actual 1  1338 ObservationValue a double   NULL file 2  1339 ObservationValue a double   NULL row 3  1340 ObservationValue a double   NULL col 4  1341 ObservationValue a double   NULL expected 5  1342 ObservationValue a double   NULL actual # ... with 1 more variables: file <chr>
    ## ... ................. ... ........................................ ........ ........................................ ...... ........................................ .... ........................................ ... ........................................ ... ........................................ ........ ........................................ ...... .......................................
    ## See problems(...) for more details.

    ## Parsed with column specification:
    ## cols(
    ##   PanelName = col_character(),
    ##   LabPanelGuid = col_character(),
    ##   LabResultGuid = col_character(),
    ##   ObservationYear = col_integer(),
    ##   Status = col_character()
    ## )

    ## Parsed with column specification:
    ## cols(
    ##   LabResultGuid = col_character(),
    ##   UserGuid = col_character(),
    ##   PatientGuid = col_character(),
    ##   TranscriptGuid = col_character(),
    ##   PracticeGuid = col_character(),
    ##   FacilityGuid = col_character(),
    ##   ReportYear = col_integer(),
    ##   AncestorLabResultGuid = col_character()
    ## )

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_character(),
    ##   ReportYear = col_integer(),
    ##   ObservationYear = col_integer(),
    ##   `ObservationYear:1` = col_integer(),
    ##   IsAbnormalValue = col_integer()
    ## )

    ## See spec(...) for full column specifications.

    ## Parsed with column specification:
    ## cols(
    ##   MedicationGuid = col_character(),
    ##   PatientGuid = col_character(),
    ##   MedicationNdcCode = col_character(),
    ##   MedicationName = col_character(),
    ##   MedicationStrength = col_character(),
    ##   Schedule = col_character(),
    ##   DiagnosisGuid = col_character(),
    ##   UserGuid = col_character()
    ## )

    ## Parsed with column specification:
    ## cols(
    ##   PatientGuid = col_character(),
    ##   dmIndicator = col_integer(),
    ##   Gender = col_character(),
    ##   YearOfBirth = col_integer(),
    ##   State = col_character(),
    ##   PracticeGuid = col_character()
    ## )

    ## Parsed with column specification:
    ## cols(
    ##   PatientConditionGuid = col_character(),
    ##   PatientGuid = col_character(),
    ##   ConditionGuid = col_character(),
    ##   CreatedYear = col_integer()
    ## )

    ## Parsed with column specification:
    ## cols(
    ##   PatientSmokingStatusGuid = col_character(),
    ##   PatientGuid = col_character(),
    ##   SmokingStatusGuid = col_character(),
    ##   EffectiveYear = col_integer()
    ## )

    ## Parsed with column specification:
    ## cols(
    ##   PatientGuid = col_character(),
    ##   dmIndicator = col_integer(),
    ##   Gender = col_character(),
    ##   YearOfBirth = col_integer(),
    ##   State = col_character(),
    ##   PracticeGuid = col_character(),
    ##   TranscriptGuid = col_character(),
    ##   `PatientGuid:1` = col_character(),
    ##   VisitYear = col_integer(),
    ##   Height = col_character(),
    ##   Weight = col_double(),
    ##   BMI = col_double(),
    ##   SystolicBP = col_character(),
    ##   DiastolicBP = col_character(),
    ##   RespiratoryRate = col_character(),
    ##   HeartRate = col_character(),
    ##   Temperature = col_character(),
    ##   PhysicianSpecialty = col_character(),
    ##   UserGuid = col_character()
    ## )

    ## Parsed with column specification:
    ## cols(
    ##   PrescriptionGuid = col_character(),
    ##   PatientGuid = col_character(),
    ##   MedicationGuid = col_character(),
    ##   PrescriptionYear = col_integer(),
    ##   Quantity = col_character(),
    ##   NumberOfRefills = col_character(),
    ##   RefillAsNeeded = col_integer(),
    ##   GenericAllowed = col_integer(),
    ##   UserGuid = col_character()
    ## )

    ## Parsed with column specification:
    ## cols(
    ##   PatientGuid = col_character(),
    ##   Gender = col_character(),
    ##   YearOfBirth = col_integer(),
    ##   State = col_character(),
    ##   PracticeGuid = col_character(),
    ##   SmokeEffectiveYear = col_integer(),
    ##   SmokingStatus_Description = col_character(),
    ##   SmokingStatus_NISTCode = col_integer()
    ## )

    ## Parsed with column specification:
    ## cols(
    ##   TranscriptGuid = col_character(),
    ##   PatientGuid = col_character(),
    ##   VisitYear = col_integer(),
    ##   Height = col_character(),
    ##   Weight = col_double(),
    ##   BMI = col_double(),
    ##   SystolicBP = col_character(),
    ##   DiastolicBP = col_character(),
    ##   RespiratoryRate = col_character(),
    ##   HeartRate = col_character(),
    ##   Temperature = col_character(),
    ##   PhysicianSpecialty = col_character(),
    ##   UserGuid = col_character()
    ## )

    ## Parsed with column specification:
    ## cols(
    ##   TranscriptAllergyGuid = col_character(),
    ##   TranscriptGuid = col_character(),
    ##   AllergyGuid = col_character()
    ## )

    ## Parsed with column specification:
    ## cols(
    ##   TranscriptDiagnosisGuid = col_character(),
    ##   TranscriptGuid = col_character(),
    ##   DiagnosisGuid = col_character()
    ## )

    ## Parsed with column specification:
    ## cols(
    ##   TranscriptMedicationGuid = col_character(),
    ##   TranscriptGuid = col_character(),
    ##   MedicationGuid = col_character()
    ## )

``` r
df <- training_data[c(10, 1, 2, 3)] %>% 
    reduce(inner_join, by = "PatientGuid") %>% 
    filter(!is.na(PatientGuid))
```

``` r
glimpse(df)
```

    ## Observations: 544,005
    ## Variables: 46
    ## $ PatientGuid           <chr> "5BC4324E-B5D5-4AAB-A000-003EACACE12F", ...
    ## $ dmIndicator           <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1...
    ## $ Gender                <chr> "F", "F", "F", "F", "F", "F", "F", "F", ...
    ## $ YearOfBirth           <int> 1939, 1939, 1939, 1939, 1939, 1939, 1939...
    ## $ State                 <chr> "OH", "OH", "OH", "OH", "OH", "OH", "OH"...
    ## $ PracticeGuid          <chr> "D1166838-9D31-46E1-9FBE-43F7A1E0D5EA", ...
    ## $ AllergyGuid.x         <chr> "BA3D76E2-1BE3-40E9-9762-9557073F9A08", ...
    ## $ AllergyType.x         <chr> "Medication", "Medication", "Medication"...
    ## $ StartYear.x           <int> 2011, 2011, 2011, 2011, 2011, 2011, 2011...
    ## $ ReactionName.x        <chr> "Dizziness/Lightheadedness", "Dizziness/...
    ## $ SeverityName.x        <chr> "Mild", "Mild", "Mild", "Mild", "Mild", ...
    ## $ MedicationNdcCode.x   <dbl> 12280039630, 12280039630, 12280039630, 1...
    ## $ MedicationName.x      <chr> "SEROquel (QUEtiapine) oral tablet", "SE...
    ## $ UserGuid.x            <chr> "3A1CD84C-0CF9-4EBE-B86E-A1C11D75F0ED", ...
    ## $ MedicationGuid        <chr> "90BE6EC5-12A0-411C-A103-03D24F2F6BAF", ...
    ## $ MedicationNdcCode.y   <chr> "00029485120", "00029485120", "000294851...
    ## $ MedicationName.y      <chr> "Relafen (nabumetone) oral tablet", "Rel...
    ## $ MedicationStrength    <chr> "500 mg", "500 mg", "500 mg", "500 mg", ...
    ## $ Schedule              <chr> "NULL", "NULL", "NULL", "NULL", "NULL", ...
    ## $ DiagnosisGuid.x       <chr> "64AD41F1-EE76-43EF-8165-571D35255B7E", ...
    ## $ UserGuid.y            <chr> "DDEA5F80-8F6F-470D-8DAB-4AC49A9BD7CF", ...
    ## $ PrescriptionGuid      <chr> "B725E6D7-C1ED-4B4F-B3CA-34CFFFA805CC", ...
    ## $ `PatientGuid:1`       <chr> "5BC4324E-B5D5-4AAB-A000-003EACACE12F", ...
    ## $ `MedicationGuid:1`    <chr> "90BE6EC5-12A0-411C-A103-03D24F2F6BAF", ...
    ## $ PrescriptionYear      <int> 2010, 2010, 2010, 2010, 2010, 2010, 2010...
    ## $ Quantity              <chr> "60", "60", "60", "60", "60", "60", "60"...
    ## $ NumberOfRefills       <chr> "5", "5", "5", "5", "5", "5", "5", "5", ...
    ## $ RefillAsNeeded        <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0...
    ## $ GenericAllowed        <int> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1...
    ## $ `UserGuid:1`          <chr> "DDEA5F80-8F6F-470D-8DAB-4AC49A9BD7CF", ...
    ## $ AllergyGuid.y         <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
    ## $ `PatientGuid:2`       <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
    ## $ AllergyType.y         <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
    ## $ StartYear.y           <int> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
    ## $ ReactionName.y        <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
    ## $ SeverityName.y        <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
    ## $ `MedicationNdcCode:1` <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
    ## $ `MedicationName:1`    <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
    ## $ `UserGuid:2`          <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
    ## $ DiagnosisGuid.y       <chr> "8D45D33E-59D9-4942-B38D-00FAA2357BBE", ...
    ## $ ICD9Code              <chr> "V03.82", "956.0", "380.4", "569.3", "70...
    ## $ DiagnosisDescription  <chr> "Need for prophylactic vaccination again...
    ## $ StartYear             <int> 2011, 0, 0, 2011, 2011, 0, 0, 0, 0, 0, 2...
    ## $ StopYear              <chr> "NULL", "NULL", "NULL", "NULL", "NULL", ...
    ## $ Acute                 <int> 1, 0, 1, 1, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0...
    ## $ UserGuid              <chr> "DDEA5F80-8F6F-470D-8DAB-4AC49A9BD7CF", ...
