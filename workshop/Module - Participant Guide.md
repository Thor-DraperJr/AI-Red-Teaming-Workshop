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

![Deploy](./images/deploytemplatedirections.png)
