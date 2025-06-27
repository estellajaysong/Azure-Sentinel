# Flashpoint CCM-E

## Overview

The key function of this solution is to retrieve enterprise compromised credentials data from Flashpoint's CCM-E API and store it in an Azure Log Analytics Workspace table using Microsoft cloud native features.

## Requirements

- [Flashpoint API Token](https://app.flashpoint.io/tokens)
- [Microsoft Azure](https://azure.microsoft.com/)
- [Microsoft Sentinel](https://azure.microsoft.com/products/microsoft-sentinel/)
- Contributor role with User Access Administrator role on the Microsoft Sentinel Resource Group **or**
Owner on the Microsoft Sentinel Resource Group  

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
6. Click `connect`
7. You should see the events populate once data begins ingesting. Note: It may take up to 30 minutes to see data begin ingesting. Credentials will continue to sync every hour onwards