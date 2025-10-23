# Azure AI Red Team Participant Guide

# Prerequisites to meet

 - [ ] Azure subscription is ideally in a Sandbox Management Group, with
       less restrictive policies
       
 - [ ] Azure Subscription has [Defender for AI Services turned
       on](https://learn.microsoft.com/en-us/azure/defender-for-cloud/ai-onboarding#enable-threat-protection-for-ai-services-1)
       
 - [ ] Attendee has access to their own Azure Subscription and can
       deploy resources to it
       
 - [ ] Azure Subscription has capacity to deploy Azure AI Foundry in
       East US 2
       
 - [ ] Azure Subscription has Provider Types registered [register_providers.sh](https://raw.githubusercontent.com/swiftsolves-msft/AI-Red-Teaming-Workshop/refs/heads/main/register_providers.sh)  can help here, by running in Azure Cloud Shell session

# PreDeploy Steps

1. Be sure to create new Resource Group as part of the deployment
2. Obtain your current user's Microsoft Entra ID (AAD) Object ID using Cloud Shell (Bash). You will use this in the deployment parameter **User Object Id**. ```az ad signed-in-user show --query id -o tsv```
3. Ensure you deploy to East US 2 region, only a select few regions are supported

![Deploy](/images/deploytemplatedirections.png)

# Participant ML Notebook Setup Instructions

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

2. Authenticate using the URL link and Device code in the terminal
3.	Afterwards you should have a .env file created and populated 

![Launch](/images/runsh.png)

## Execute installs, imports, and credential login

1. Open the AIRT-AiHML.ipynb file.

![Launch](/images/airtnotebook.png)

2. Run the first install cell. A pop‑up prompts you to select the Python kernel (choose your compute instance).

![Launch](/images/newkernel.png)

3. You should now see the ML compute selected and the first run succeeded (green check mark). The top-right 'Select Kernel' indicator updates.

![Launch](/images/cellrun.png)

4. Continue executing the remaining setup cells. You may receive an error that can safely be ignored.

![Launch](/images/error.png)

![Launch](/images/import.png)

5.	During the login step, you should see "Managed Identity OK".

![Launch](/images/msi.png)

6.	Running the next step ensures the .env file is found and loaded properly. If needed, adjust the pathing when variables do not load.

![Launch](/images/loadenv.png)

# Run AI red team evaluations agent

**Avg execution time (section): ~25 min**

These instructions cover running the remaining AIRT.ipynb cells where the AI Red Teaming Agent conducts basic, intermediate, advanced, and custom prompt attacks against the target model in your Azure AI project.

## Prerequisites

1.	Azure AI project deployed (hub + project) with system‑assigned managed identity.
2.	Default blob storage connection (workspaceblobstore) set to Microsoft Entra ID-based (see Module 1 if you need to toggle it). No SAS or account keys required.
3.	OpenAI deployment (e.g., gpt-4o-mini) accessible with your identity (Cognitive Services OpenAI User role).
4.	Python environment with azure-ai-evaluation[redteam] installed (done in earlier notebook cells).
5.	Region supported for AI Red Teaming Agent (preview) – confirm your project region is in the current preview list.

## Key Microsoft Documentation

- AI Red Teaming Agent overview (preview): https://learn.microsoft.com/azure/ai-foundry/concepts/ai-red-teaming-agent
- Run scans with the AI Red Teaming Agent: https://learn.microsoft.com/azure/ai-foundry/how-to/develop/run-scans-ai-red-teaming-agent
- View AI Red Teaming results: https://learn.microsoft.com/azure/ai-foundry/how-to/view-ai-red-teaming-results
- Observability & evaluation stages: https://learn.microsoft.com/azure/ai-foundry/concepts/observability#the-three-stages-of-genaiops-evaluation
- Risk & safety evaluators: https://learn.microsoft.com/azure/ai-foundry/concepts/evaluation-evaluators/risk-safety-evaluators

## Workflow summary

1.	Generate / specify attack objectives (risk categories, counts).
2.	Launch AI Red Teaming Agent scan (PyRIT-powered) from the notebook.
3.	Monitor scan progress (optional logging output).
4.	Retrieve evaluation metrics (attack success rate, per-category breakdown).
5.	Inspect conversation-level artifacts for successful vs. failed attacks.
6.	Iterate with additional categories / custom prompts.

# Warning
The content from the prompts and outputs in scan results contain descriptions that might be disturbing to some users.

### Basic attack

The Basic attack focuses on default risk categories (violence, sexual, hate & unfairness, self-harm) with a low prompt count to validate the pipeline end-to-end.

Results from the execution of the cell should indicate Completed Tasks and Evaluation Results being saved and uploaded.

![Launch](/images/basicattack.png)

Be sure to review the risk_categories available in the array. While Basic focuses on Violence and Hateful/Unfair Content, there are others [documented here](https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/evaluation-evaluators/risk-safety-evaluators).

If you want to add to the risk_categories array, you can add elements by typing RiskCategory. and choosing other categories from the IntelliSense suggestions list.

![Launch](/images/riskcategory.png)

### Intermediate attack

The Intermediate attack increases the objectives count and optionally introduces additional attack strategies (e.g., obfuscation, role-play) to probe guardrails more thoroughly.

Be sure to review the attack_strategies available in the array. While Basic only used the Flip strategy, Intermediate employs many other techniques to manipulate and trick the model behavior into the categories being used.

![Launch](/images/intattack.png)

Just like the risk_categories array in the Basic attack, if you want to add additional attack strategies to the Intermediate attack, you can add elements to the attack_strategies array by typing AttackStrategy. and choosing other strategy types from the IntelliSense suggestions list.

![Launch](/images/attackstrat.png)

