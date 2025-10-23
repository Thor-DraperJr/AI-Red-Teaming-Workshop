# Azure AI Red Team Participant Guide

## Prerequisites to meet

 - [ ] Azure subscription is ideally in a Sandbox Management Group, with
       less restrictive policies
       
 - [ ] Azure Subscription has [Defender for AI Services turned
       on](https://learn.microsoft.com/en-us/azure/defender-for-cloud/ai-onboarding#enable-threat-protection-for-ai-services-1)
       
 - [ ] Attendee has access to their own Azure Subscription and can
       deploy resources to it
       
 - [ ] Azure Subscription has capacity to deploy Azure AI Foundry in
       East US 2
       
 - [ ] Azure Subscription has Provider Types registered [register_providers.sh](https://raw.githubusercontent.com/swiftsolves-msft/AI-Red-Teaming-Workshop/refs/heads/main/register_providers.sh)  can help here, by running in Azure Cloud Shell session

## PreDeploy Steps

1. Be sure to create new Resource Group as part of the deployment
2. Obtain your current user's Microsoft Entra ID (AAD) Object ID using Cloud Shell (Bash). You will use this in the deployment parameter **User Object Id**. ```az ad signed-in-user show --query id -o tsv```
3. Ensure you deploy to East US 2 region, only a select few regions are supported

![Deploy](/images/deploytemplatedirections.png)

**Avg setup time: ~10 min**

These instructions configure the ML notebook environment needed to execute AI red team evaluations from AIRT-AiHML.ipynb.
Execution environment note: 

This workshop is intentionally designed to run the notebooks on the Azure Machine Learning compute instance that is deployed (remote Jupyter kernel), not your local laptop Python environment. Running on the managed compute ensures:

 - Consistent, pre-provisioned VM size & dependencies across
   participants
- Managed identity & RBAC access to storage and project resources
   without exposing keys locally
 - Isolation from local machine package/version conflicts
- Easier cleanup (stop / delete compute when finished) If you still
   choose to run locally: (1) create a virtual env with Python >=3.10,
   (2) install dependencies matching the first install cell, (3)
   replicate the .env variables, and (4) ensure your identity or
   key-based auth has the same roles. Local runs may diverge from
   screenshots and are not the validated workshop path.

## Launch VS Code Web

1.	Open URL to:  https://ml.azure.com/workspaces and navigate to the Machine Learning workspace Azure resource by clicking the Name.
 
![Launch](/images/launchmlworkspace.png)

2. In Machine Learning Studio, Goto Compute , ensure the VM is running and launch VS Code Web

![Launch](/images/launchvscodeweb.png)

3. Allow Extension to use Remote and Authenticate

![Launch](/images/allowext.png)

4. Trust the Authors dialog box

![Launch](/images/trust.png)

5. Be sure to download the workshop files from GitHub and unzip and enter the Shared root folder into the workshop and drag *files* folder into the VS Code

![Launch](/images/downloadzip.png)

![Launch](/images/copyover.png)

![Launch](/images/copied.png)

6. Right Click on *files* folder in Left Navigation pane, and click Open in Integrated terminal

![Launch](/images/openterminal.png)

7.	click on generate-env.sh

## (Script) Create .env and populate information

1. execute ```./generate-env.sh```

![Launch](/images/runsh.png)

2.   

  

