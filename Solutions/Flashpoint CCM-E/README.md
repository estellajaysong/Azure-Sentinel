# Flashpoint CCM-E

## Overview

The key function of this Solution is to retrieve enterprise compromised credentials data from Flashpoint CCM-E API and store it in an Azure Log Analytics Workspace table using Microsoft cloud native features.

## Azure services needed

### Required

- [Flashpoint API Token](https://app.flashpoint.io/tokens)
- [Microsoft Azure](https://azure.microsoft.com/)
- [Microsoft Sentinel](https://azure.microsoft.com/products/microsoft-sentinel/)
- Contributor role with User Access Administrator role on the Microsoft Sentinel Resource Group <br>
**or**
- Owner on the Microsoft Sentinel Resource Group  

## Manual Installation using the ARM template

1. Install a data connector using an ARM template [here](https://portal.azure.com/#create/Microsoft.Template)
2. Click `Build your own template in the editor`
3. Copy the contents of `mainTemplate.json` in our repo and paste it into the text box
4. Click `Save`
5. Fill in `Custom deployment` details, including `Resource group`, `Workspace-location`, and `Workspace`
6. Click `Review + create`
7. Click `Create`
8. You should be able to see the deployment details

## Set up the connector
1. Once deployment has completed, go to `Services` > `Microsoft Sentinel`
2. Click your workspace
3. Click `Configuration` > `Data connectors` in the navigation
4. Click `Flashpoint CCM-E` > `Open connector page`
5. Fill in your Flashpoint API token and filters for the credential sightings you want to ingest
6. Click connect
7. You should see the events populate once data begins ingesting. Note: It may take up to 30 minutes to see data begin ingesting. Credentials will continue to sync every hour onwards

</details>

## Deployed Resources

The 1Password Solution for Microsoft Sentinel is comprised of following Azure resources:

> Click on the topics below to fold them out.

<details>

<summary>Resource Group</summary>
<br/>

### **Resource Group**

The Azure resource group is used as a container to group a set of Azure resources that share the same lifecycle.
> NOTE: Known limitation is that the Solution can only be deployed within the same `resourcegroup` as where Microsoft Sentinel is hosted.

</details>

<details>

<summary>Function App</summary>
<br/>

### **FunctionApp**

The Azure FunctionApp runs on top of an Azure App Service and is used to host the _PowerShell_ function to query the 1Password API endpoint. The Azure FunctionApp has the following components:

```powershell
|- WWWROOT
|-|- Modules
|-|-|- HelperFunctions.psm1
|-|- function
|-|-|- function.json
|-|-|- run.ps1
|-|- host.json
|-|- profile.ps1
|-|- requirements.psd1
```

The ```HelperFunctions.psm1``` module is used to simplify the FunctionApp code and handles security related tasks like:

- Query the 1Password Events API endpoint
- Send the data to the Data Collection Rule endpoint
- Set and retrieve the cursor and timestamp to a storage account

</details>

<details>

<summary>Key Vault</summary>
<br/>

### **Key Vault**

The Azure Key Vault resource is used to securely store certain sensitive or secret values used in the 1Password Solution for Microsoft Sentinel. 
Because of the sensitivity of the secrets in the Key Vault, access is restricted to the Managed Identity (MSI) of the FunctionApp.
Secrets that reside in the vault are:

- APIKey (1password)
- functionAppPackage (location to zip package hosting the function)
- dataCollectionEndpoint (endpoint for uploading 1Password logs)

</details>

<details>

<summary>Storage Account</summary>
<br/>

### **Storage Account**

The Storage Account resources is used to store logs and properties of the Azure FunctionApp.

</details>

<details>

<summary>Application Insights</summary>
<br/>

### **Application Insights**

The Application Insights instance is used for collecting telemetry of the Azure FunctionApp.
This provides visibility into the availability, performance, and usage patterns of the FunctionApp.

</details>

<details>

<summary>Data Collection Rule</summary>
<br/>

### **Data Collection Rule (DCR)**

The Data Collection Rule is attached to a _data collection endpoint_ and a Log Analytics table.
The Managed Identity (MSI) of the FunctionApp is used to authenticate against the data collection endpoint.

</details>

<details>

<summary>Custom Table</summary>
<br/>

## **Custom Table**

During deployment, a custom table with the name "OnePasswordEventLogs_CL" is created in the Log Analytics workspace.

</details>

<details>

<summary>Role Assignment</summary>
<br/>

### **Role Assignment**

The identity used to send the data to the Data Collection Endpoint needs to have the _Monitoring Metrics Publisher_ role on the Data Collection Rule (DCR). 
> NOTE: I can take up to 30 minutes after deployment before the first data is received by the table. <br>

</details>

## Implementation resources

The 1Password Solution for Microsoft Sentinel is deployed from the Data Connector in sentinel. You must create the Data Connector in order to deploy the 1Password Solution. 
> Note: In the 1Password (Preview) Solution the installation in done using an ARM (Azure Resource Manager) template.<br>Once the Solution is in GA (general availability) it will be installed from the Microsoft Sentinel content hub.


## Post Deployment steps

- N/A
