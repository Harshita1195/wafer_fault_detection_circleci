# Wafer Fault Detection

## Project Overview
The Wafer Fault Detection project aims to develop a machine learning model to predict the operational status of semiconductor wafers based on sensor data. Each wafer can be classified as:

- **+1**: Working condition (no replacement needed)
- **-1**: Faulty (replacement required)

## Data Description
- The input data consists of multiple sets of files, each containing:
  - Wafer names
  - 590 columns of sensor values
  - A final column indicating the wafer status ("Good" or "Bad")
- A schema file is also required, which contains:
  - File names
  - Date and time value lengths
  - Number of columns
  - Column names and their data types

## Data Validation
1. **Name Validation**: Checks file names against a regex pattern defined in the schema, validating date and time lengths.
   - Files that pass are moved to `Good_Data_Folder`; others go to `Bad_Data_Folder`.
2. **Column Validation**: Ensures the correct number and names of columns are present. Mismatched files are moved to `Bad_Data_Folder`.
3. **Data Type Validation**: Validates that data types match those specified in the schema during database insertion.
4. **Null Values**: Files with columns having all NULL values are discarded and moved to `Bad_Data_Folder`.

## Data Insertion in Database
- A database is created (or connected to) to store valid data.
- A table named `Good_Data` is created for storing validated files. Existing tables are used to insert new data without creating duplicates.
- Files in the `Good_Data_Folder` are inserted, with invalid files moved to `Bad_Data_Folder`.

## Model Training
1. **Data Export**: Data is exported from the database as a CSV file for model training.
2. **Data Preprocessing**:
   - Null values are imputed using KNN imputer.
   - Columns with zero standard deviation are removed.
3. **Clustering**: KMeans clustering is applied to the preprocessed data to identify optimum clusters.

## Containerization and CI/CD Setup
- A **Dockerfile** is created to containerize the application, specifying the use of Python 3.7 and dependencies from `requirements.txt`.
- A **Procfile** is included for deployment using Gunicorn.
- **CircleCI Configuration**: A `.circleci/config.yml` file is established for continuous integration and deployment, which includes:
  - Build and test steps with Docker support.
  - Environment variable settings for Heroku and Docker Hub.
  - Steps for pushing the application to Heroku after testing.

## Git and Environment Setup
- A Git repository is initialized, and the code is pushed to a remote repository.
- Necessary environment variables for CircleCI, Docker Hub, and Heroku are configured.

## Conclusion
The Wafer Fault Detection project involves a comprehensive approach to validate, process, and analyze wafer sensor data to build a reliable predictive model. The incorporation of CI/CD practices ensures a smooth deployment pipeline, facilitating updates and maintenance.
