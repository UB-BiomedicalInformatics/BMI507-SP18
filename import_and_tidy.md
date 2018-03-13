Kaggle Data Import & Tidy
================

Load Required Packages
----------------------

``` r
library(readr)      # for reading data from flat files
library(stringr)    # for performing advanced string operations
library(purrr)      # for functional programming using the `map` functions
library(dplyr)      # provides `%>%` operator
```

File and Object Names
---------------------

Keeping track of directory, file, and other object names is important to an orderly and reproducible analysis. It is important to do so from the project's very beginning so we don't get lost but also because patterns in those names can make our code easier to create and follow later on.

-   The data is provided in two directories:
    -   one for training (`data/sample_data/train_data/`)
    -   one for testing (`data/sample_data/test_data/`)
-   Using the `list.files()` function, we can see that each directory contains 19 individual (Comma Separated Value) [CSV](https://en.wikipedia.org/wiki/Comma-separated_values) files whose names follow a recognizable pattern:
    -   each file in the `train_data` directory starts with `training_` and ends with `.csv`
    -   each file in the `test_data` directory starts with `test_` and ends with `.csv`

``` r
training_files <- list.files(path = "data/sample_data/train_data/")
testing_files <- list.files(path = "data/sample_data/test_data/")

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
    ## [10] "training_patientCondition.csv"    
    ## [11] "training_patient.csv"             
    ## [12] "training_patientSmokingStatus.csv"
    ## [13] "training_patientTranscript.csv"   
    ## [14] "training_prescription.csv"        
    ## [15] "training_smoke.csv"               
    ## [16] "training_transcriptAllergy.csv"   
    ## [17] "training_transcript.csv"          
    ## [18] "training_transcriptDiagnosis.csv" 
    ## [19] "training_transcriptMedication.csv"

``` r
testing_files
```

    ##  [1] "test_allergy.csv"              "test_allMeds.csv"             
    ##  [3] "test_diagnosis.csv"            "test_immunization.csv"        
    ##  [5] "test_labObservation.csv"       "test_labPanel.csv"            
    ##  [7] "test_labResult.csv"            "test_labs.csv"                
    ##  [9] "test_medication.csv"           "test_patientCondition.csv"    
    ## [11] "test_patient.csv"              "test_patientSmokingStatus.csv"
    ## [13] "test_patientTranscript.csv"    "test_prescription.csv"        
    ## [15] "test_smoke.csv"                "test_transcriptAllergy.csv"   
    ## [17] "test_transcript.csv"           "test_transcriptDiagnosis.csv" 
    ## [19] "test_transcriptMedication.csv"

The `list.files()` function returns a character vector of file names. Each file name in the vector has an index that our other functions can refer to. Using bracket notation, we can access each element of the vector:

``` r
training_files[1]
```

    ## [1] "training_allergy.csv"

Or we can access a range of elements at once:

``` r
testing_files[11:13]
```

    ## [1] "test_patient.csv"              "test_patientSmokingStatus.csv"
    ## [3] "test_patientTranscript.csv"

We will use these file names as inputs for the `read_csv` funtion from the package `readr`.

Read Files Into Working Memory
------------------------------

We will use a combination of functions to import or "read" the source files into working memory. There are a number of ways to do this but in the interest of organization we will use a combination of `read_csv()`, `paste0()`, and `map()`.

``` r
training_data <- training_files %>% 
    map(~paste0("data/sample_data/train_data/", .x)) %>% 
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
    ## row # A tibble: 5 x 5 col     row col              expected actual file                              expected   <int> <chr>            <chr>    <chr>  <chr>                             actual 1  1338 ObservationValue a double NULL   'data/sample_data/train_data/tra… file 2  1339 ObservationValue a double NULL   'data/sample_data/train_data/tra… row 3  1340 ObservationValue a double NULL   'data/sample_data/train_data/tra… col 4  1341 ObservationValue a double NULL   'data/sample_data/train_data/tra… expected 5  1342 ObservationValue a double NULL   'data/sample_data/train_data/tra…
    ## ... ................. ... .......................................................................... ........ .......................................................................... ...... .......................................................................... .... .......................................................................... ... .......................................................................... ... .......................................................................... ........ ..........................................................................
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
    ##   PatientConditionGuid = col_character(),
    ##   PatientGuid = col_character(),
    ##   ConditionGuid = col_character(),
    ##   CreatedYear = col_integer()
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
    ##   TranscriptAllergyGuid = col_character(),
    ##   TranscriptGuid = col_character(),
    ##   AllergyGuid = col_character()
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
testing_data <- testing_files %>% 
    map(~paste0("data/sample_data/test_data/", .x)) %>% 
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
    ##   MedicationNDCCode = col_double(),
    ##   MedicationName = col_character(),
    ##   UserGuid = col_character()
    ## )

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_character(),
    ##   PrescriptionYear = col_integer(),
    ##   RefillAsNeeded = col_integer(),
    ##   GenericAllowed = col_integer()
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
    ##   HL7Codingsystem = col_character(),
    ##   ObservationValue = col_character(),
    ##   Units = col_character(),
    ##   ReferenceRange = col_character(),
    ##   AbnormalFlags = col_character(),
    ##   ResultStatus = col_character(),
    ##   ObservationYear = col_integer(),
    ##   UserGuid = col_character(),
    ##   SsAbnormalValue = col_integer()
    ## )

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
    ##   SsAbnormalValue = col_integer()
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
    ##   PatientConditionGuid = col_character(),
    ##   PatientGuid = col_character(),
    ##   ConditionGuid = col_character(),
    ##   CreatedYear = col_integer()
    ## )

    ## Parsed with column specification:
    ## cols(
    ##   PatientGuid = col_character(),
    ##   Gender = col_character(),
    ##   YearOfBirth = col_integer(),
    ##   State = col_character(),
    ##   PracticeGuid = col_character()
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
    ##   Gender = col_character(),
    ##   YearOfBirth = col_integer(),
    ##   State = col_character(),
    ##   PracticeGuid = col_character(),
    ##   TranscriptGuid = col_character(),
    ##   `PatientGuid:1` = col_character(),
    ##   VisitYear = col_integer(),
    ##   Height = col_character(),
    ##   Weight = col_double(),
    ##   BMI = col_character(),
    ##   SystolicBP = col_integer(),
    ##   DiastolicBP = col_integer(),
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
    ##   TranscriptAllergyGuid = col_character(),
    ##   TranscriptGuid = col_character(),
    ##   AllergyGuid = col_character()
    ## )

    ## Parsed with column specification:
    ## cols(
    ##   TranscriptGuid = col_character(),
    ##   PatientGuid = col_character(),
    ##   VisitYear = col_integer(),
    ##   Height = col_character(),
    ##   Weight = col_double(),
    ##   BMI = col_character(),
    ##   SystolicBP = col_integer(),
    ##   DiastolicBP = col_integer(),
    ##   RespiratoryRate = col_character(),
    ##   HeartRate = col_character(),
    ##   Temperature = col_character(),
    ##   PhysicianSpecialty = col_character(),
    ##   UserGuid = col_character()
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

The result is two `list` objects, one for training data and the other for testing data, each of which contains 19 `data.frame` objects - one for each CSV file. To make these easier to keep track of, we are going to assign each `data.frame` in the lists a name by cleaning up the file names.

``` r
training_files_names <- str_replace(training_files, "training_", "")
training_files_names <- str_replace(training_files_names, "\\.csv", "")
names(training_data) <- training_files_names

testing_files_names <- str_replace(testing_files, "test_", "")
testing_files_names <- str_replace(testing_files_names, "\\.csv", "")
names(testing_data) <- testing_files_names
```

``` r
names(training_data)
```

    ##  [1] "allergy"              "allMeds"              "diagnosis"           
    ##  [4] "immunization"         "labObservation"       "labPanel"            
    ##  [7] "labResult"            "labs"                 "medication"          
    ## [10] "patientCondition"     "patient"              "patientSmokingStatus"
    ## [13] "patientTranscript"    "prescription"         "smoke"               
    ## [16] "transcriptAllergy"    "transcript"           "transcriptDiagnosis" 
    ## [19] "transcriptMedication"

``` r
names(testing_data)
```

    ##  [1] "allergy"              "allMeds"              "diagnosis"           
    ##  [4] "immunization"         "labObservation"       "labPanel"            
    ##  [7] "labResult"            "labs"                 "medication"          
    ## [10] "patientCondition"     "patient"              "patientSmokingStatus"
    ## [13] "patientTranscript"    "prescription"         "smoke"               
    ## [16] "transcriptAllergy"    "transcript"           "transcriptDiagnosis" 
    ## [19] "transcriptMedication"

Now our data objects are neatly arranged. Scripts we write to manipulate one of these lists can easily be applied to the other, ensuring uniformity across operations and making it easier to trace errors back to their sources.

Save Prepared Data
------------------

We will save each of the prepared lists as `.RDS` files, which are easily read by R. Another option would be to save each `data.frame` in the lists as its own `.csv` file.

``` r
saveRDS(training_data, "data/project_data/training.RDS")
saveRDS(testing_data, "data/project_data/testing.RDS")
```
