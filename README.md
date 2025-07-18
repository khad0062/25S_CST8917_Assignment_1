# CST8917 Assignment 1: Durable Workflow for Image Metadata Processing

## Objective
Build a serverless image metadata processing pipeline using Azure Durable Functions in Python. This solution uses blob triggers, activity functions, and output bindings to simulate a real-world event-driven system.

## YouTube Demo
[Youtube video Link](https://youtu.be/YTMCbmk4m_0)

## Workflow Overview
1. **Blob Trigger (Client Function):**
   - Triggers on new image uploads (.jpg, .png, .gif) to the `images-input` container.
   - Starts the orchestration.
2. **Orchestrator Function:**
   - Calls an activity function to extract metadata from the image.
   - Calls a second activity function to store that metadata in Azure SQL DB.
3. **Activity Function – Extract Metadata:**
   - Extracts file name, file size (KB), width, height, and format.
4. **Activity Function – Store Metadata:**
   - Stores the extracted metadata in Azure SQL Database.

## Azure CLI Commands Used


### 1. Create Resource Group
Creates a logical container for all your resources:
```sh
az group create --name cst8917-rg --location canadacentral
```

### 2. Create Storage Account

```sh
az storage account create --name cst8917storage --resource-group cst8917-rg --location canadacentral --sku Standard_LRS
```

### 3. Create Blob Container
Creates the container for image uploads:
```sh
az storage container create --name images-input --account-name cst8917storage
```

### 4. Create Azure SQL Server
Creates the logical SQL server (replace password with a strong one):
```sh
az sql server create --name cst8917sql --resource-group cst8917-rg --location canadacentral --admin-user sqladminuser --admin-password <yourpassword>
```

### 5. Create Azure SQL Database
Creates the database to store image metadata:
```sh
az sql db create --resource-group cst8917-rg --server cst8917sqlserver --name cst8917sql --service-objective S0
```

### 6. Configure SQL Server Firewall Rule
Allows Azure services to access the SQL server:
```sh
az sql server firewall-rule create --resource-group cst8917-rg --server cst8917sqlserver --name AllowAzure --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

## Project Structure
- `function_app.py`: Main Durable Functions app with all triggers and activities.
- `host.json`, `local.settings.json`: Azure Functions configuration files.
- `requirements.txt`: Python dependencies.

## How to Run Locally
1. Install dependencies:
   ```
   pip install -r requirements.txt
   ```
2. Start Azure Functions host:
   ```
   func host start
   ```
3. Upload an image to the `images-input` blob container to trigger the workflow.




