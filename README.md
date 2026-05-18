# End-to-End-Data-engineering-Project


Adventure Works Data Engineering Project

Project Overview

  This is an end-to-end Data Engineering project built using the Adventure Works dataset.
  The project demonstrates how to:
  
  Extract data from GitHub (HTTP source)
  Store data in Azure Data Lake
  Transform data using Azure Databricks
  Build a Data Warehouse using Azure Synapse Analytics
  Create dashboards and visualizations in Power BI

The project follows the Medallion Architecture approach using:

  Bronze Layer → Raw Data
  Silver Layer → Transformed Data
  Gold Layer → Data Warehouse / Analytics Layer
  Architecture Flow

GitHub CSV Files → Azure Data Factory → Azure Data Lake (Bronze) → Azure Databricks (Silver) → Azure Synapse (Gold) → Power BI

  Technologies Used
  Microsoft Azure
  Azure Data Factory (ADF)
  Azure Data Lake Storage Gen2
  Azure Databricks
  Azure Synapse Analytics
  Power BI
  GitHub (HTTP Data Source)
  Step 1: Azure Setup
  Create Resource Group

First, create a Resource Group in Azure.
All services and resources for this project will be created inside this Resource Group.

  Create Azure Data Lake Storage
  
  Create a Storage Account and enable:
  
  Hierarchical Namespace (required for Data Lake Gen2)
  
  Inside the Data Lake, create the following containers:

Container	Purpose
  Bronze	Store raw data from GitHub
  Silver	Store transformed data from Databricks
  Gold	Store final warehouse/analytics data
  Create Azure Services

Create the following services inside the same Resource Group:

  Azure Data Factory
  Azure Databricks
  Azure Synapse Analytics
  Step 2: Build Data Pipeline in Azure Data Factory
  Source Data

The source data consists of multiple CSV files stored in a GitHub repository.

Create Linked Services and Datasets

In Azure Data Factory:

  Create Linked Services
  Create Datasets for the CSV files
  
  Since multiple CSV files are loaded dynamically, parameters are required.
  
  Dynamic Parameters Used

The following parameters change for each file:

  Relative URL
  Folder Name
  File Name
  Create Pipeline
  1. Lookup Activity
  Create a JSON configuration file
  Upload the JSON file into a new container in the Data Lake
  Use the Lookup Activity to read the JSON file
  2. ForEach Activity
  Connect the output value from Lookup Activity
  Loop through each file dynamically

4. Copy Activity

  Use Copy Activity to load files from GitHub into the Bronze container.
  
  Example parameter mapping:
  
  P_rel_url = @item().P_rel_url
  
  This allows dynamic loading of multiple files.

<img width="523" height="146" alt="image" src="https://github.com/user-attachments/assets/0f0e4ecf-d9e7-47d7-ae8b-0b78f5351857" />

Step 3: Data Transformation Using Azure Databricks
  Create Databricks Workspace
  
  Create an Azure Databricks workspace inside the Resource Group.
  
  Create Cluster
  
  Inside Databricks:
  
  Go to Compute
  Create a new Cluster
  Connect Databricks to Azure Data Lake

To access Data Lake from Databricks:

  Create Microsoft Entra ID App Registration
  Go to Azure Portal
  Open Microsoft Entra ID
  Go to App Registrations
  Create a New Registration

  Copy the following values:
  
  Client ID (Application ID)
  Directory ID (Tenant ID)
  Create Secret
  Open Certificates & Secrets
  Create a New Client Secret
  Copy the Secret Value immediately (it disappears later)
  Assign Permissions

Go to the Data Lake Storage Account:

  Open IAM (Access Control)
  Add Role Assignment
  Select:
  Storage Blob Data Contributor
  Add the Service Principal created in Microsoft Entra ID
  Step 4: Use Credentials in Azure Databricks

Use the following credentials in Databricks to connect with Azure Data Lake:

  Client ID
  Tenant ID
  Client Secret
  Storage Account Details
  
  Databricks will:
  
  Read data from Bronze layer
  Transform the data
  Load transformed data into Silver layer
Final Output

After transformation:

  Data is loaded into the Gold layer using Azure Synapse Analytics
  Power BI is connected to the Gold layer
  Dashboards and visualizations are created for business insights
  Project Outcome

This project demonstrates:

  End-to-end Data Engineering workflow
  Dynamic data ingestion using Azure Data Factory
  Scalable cloud data storage
  Data transformation using Databricks
  Data warehousing using Synapse Analytics
  Reporting and visualization using Power BI
