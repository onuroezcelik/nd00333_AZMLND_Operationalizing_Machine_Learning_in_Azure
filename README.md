# Operationalizing Machine Learning

In this project, a cloud-based machine learning production model is configured, deployed, and consumed in Azure. Furthermore a pipeline is created, published, and consumed.
A bank marketing dataset from https://www.kaggle.com/datasets/janiobachmann/bank-marketing-dataset is used for the model training.

## Architectural Diagram
The following steps are performed:

1. Authentication
2. Automated ML Experiment
3. Deploy the best model
4. Enable logging
5. Swagger Documentation
6. Consume model endpoints
7. Create and publish a pipeline
8. Documentation

![](images/project_main_steps.png)

## Key Steps
### 1. Authentication

   The project lab is used to complete the project. Therefore this step is skipped since I am not authorized to create a security principal.

### 2. Automated ML Experiment

   Azure Machine Learning GUI is used to create a new automated ML run.
   
   #### 2.1. Basic settings
   - Experiment name is defined at the step of "Basic Settings"
     
     ![](images/2.1_basic_settings.png)

   #### 2.2. Task type and data
   - Task type is selected as classification.
   - The data asset is registered by locally uploading the csv file from https://www.kaggle.com/datasets/janiobachmann/bank-marketing-dataset
     
     ![](images/2.2_task_type_n_data.png)

   #### 2.3. Task settings
   - The target column is selected as "deposit"
     
     ![](images/2.3_task_settings.png)

   - Additional configuration settings is done by
     - selecting "Primary metric" as "Accuracy"
     - checking "Explain best model" is enabled
       
       ![](images/2.3_task_settings_additional_configuration.png)
     
   - Limits are set as follows:
     - max trials = 25
     - max concurrent trials = 5 as expected from the objectives of the project
     - max nodes = 6
     - experiment timeout = 60 mins (1 hours) as expected from the objectives of the project
     - iteration timeout = 15 as minumum recommended duration by Azure ML studio
       
       ![](images/2.3_task_settings_limits.png)

   - Validate and test data section are configured as explained in the related lesson
     - Validation type is selected as Train-validation split with 10%
     - Test data is selected as Train-test split with 10%
       
       ![](images/2.3_task_settings_validate_test.png)
   
   #### 2.4. Compute cluster
   - A compute cluster is needed to run the automated ml models.
     - Standard_DS2_v2 is selected as optimal performance
     - minimum number of nodes is set to 1 as expected from the objectives of the project
     - maximum number of nodes is set to 6 at least for 5 concurrent runs

       ![](images/2.4_compute_cluster_settings.png)
   
   #### 2.5. Submit the job
   - After a final review of all automl settings, the job is submitted
    
     ![](images/2.5_automl_review.png)
    
   #### 2.6. Finding best model
   - As a result of submitted job, the best performing model is provided as VotingEnsemble shown below.

     ![](images/2.6_best_model.png)
    
### 3. Deployment of the Best Model
   - Starting a deployment as a web service
     The deployment of the best model is started by selecting the followng settings
     - Define a name for the deployment. I named it as "bestmodeldeploy".
     - Compute type is selected as ACI
     - Authentication is enabled

       ![](images/3_best_model_deployment.png)

   - Deployment is succeeded.
     
     ![](images/3_best_model_deployment_succeeded.png)

   - Application insights is disabled.

     ![](images/3_best_model_deployment_succeeded_app_insights_false.png)

### 4. Enabling Logging

- log.py is updated to enable Application insights by modifiying the deployment name to "bestmodeldeploy"

  ![](images/4_enable_logging.png)

- Application insights is now enabled after running log.py

  ![](images/4_application_insigts_enabled.png)

### 5. Consume Model Points
This section describes how the deployed machine learning model is accessed and tested
through its REST API endpoint.

   #### 5.1. Downloading swagger.json file
   After the model is successfully deployed, Azure Machine Learning automatically generates a `swagger.json` file that describes the REST API interface of the deployed service.
   This file contains details about the available endpoints, request and response schemas, and input data formats.

   The `swagger.json` file is downloaded from the Azure Machine Learning Studio and can be used to visualize and test the model endpoint using tools such as Swagger UI.
   This step helps verify that the endpoint is functioning correctly and that requests are formatted as expected.
   
   ![](images/5.1_downloading_swagger_json.png)

   #### 5.2. Running swagger.sh file
   The `swagger.sh` script is used to launch a local instance of **Swagger UI** to interactively explore and test the deployed model endpoint.
   Originally, port 80 was used, but it was changed to 9000
   
   ![](images/5.2_editing_swagger_bh.png)

   To run the script, the following command is executed in **Git Bash**:
   bash swagger.sh
   ![](images/5.2_running_swagger_bh.png)

   #### 5.3. Running serve.py file

   To run the script, the following command is executed in **Git Bash**:
   python serve.py
   ![](images/5.3_running_serve.png)

   #### 5.4. Swagger UI
   
   ![](images/5.4_swagger_ui.png)
   ![](images/5.4_swagger_ui_score.png)
   
   #### 5.5. Running endpoint.py file
   
   ![](images/5.5_editing_endpoint_py_1.png)
   ![](images/5.5_editing_endpoint_py_2.png)
   ![](images/5.5_running_endpoint_py.png)
  
### 6. Create and Publish a Pipeline


## Screen Recording
Due to company policy restrictions, screen recording is not permitted in the working environment.  
Therefore, a screen recording of the project execution cannot be provided.

The project functionality and workflow are instead demonstrated through detailed screenshots
and step-by-step explanations throughout this README.

## Standout Suggestions
*TODO (Optional):* This is where you can provide information about any standout suggestions that you have attempted.
