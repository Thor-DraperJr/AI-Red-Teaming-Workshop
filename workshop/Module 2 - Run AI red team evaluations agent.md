# Run AI red team evaluations agent

> **Avg execution time (section): ~15 min**

These instructions cover running the remaining `AIRT.ipynb` cells where the AI Red Teaming Agent conducts basic, intermediate, advanced, and custom prompt attacks against the target model in your Azure AI project.

## Prerequisites

1. Azure AI project deployed (hub + project) with system‑assigned managed identity.
2. Default blob storage connection (workspaceblobstore) set to **Microsoft Entra ID-based** (see Module 1 if you need to toggle it). No SAS or account keys required.
3. OpenAI deployment (e.g., `gpt-4o-mini`) accessible with your identity (Cognitive Services OpenAI User role).
4. Python environment with `azure-ai-evaluation[redteam]` installed (done in earlier notebook cells).
5. Region supported for AI Red Teaming Agent (preview) – confirm your project region is in the current preview list.

## Key Microsoft Documentation

- AI Red Teaming Agent overview (preview): <https://learn.microsoft.com/azure/ai-foundry/concepts/ai-red-teaming-agent>
- Run scans with the AI Red Teaming Agent: <https://learn.microsoft.com/azure/ai-foundry/how-to/develop/run-scans-ai-red-teaming-agent>
- View AI Red Teaming results: <https://learn.microsoft.com/azure/ai-foundry/how-to/view-ai-red-teaming-results>
- Observability & evaluation stages: <https://learn.microsoft.com/azure/ai-foundry/concepts/observability#the-three-stages-of-genaiops-evaluation>
- Risk & safety evaluators: <https://learn.microsoft.com/azure/ai-foundry/concepts/evaluation-evaluators/risk-safety-evaluators>

## Workflow summary

1. Generate / specify attack objectives (risk categories, counts).
2. Launch AI Red Teaming Agent scan (PyRIT-powered) from the notebook.
3. Monitor scan progress (optional logging output).
4. Retrieve evaluation metrics (attack success rate, per-category breakdown).
5. Inspect conversation-level artifacts for successful vs. failed attacks.
6. Iterate with additional categories / custom prompts.

> [!WARNING]
> The content from the prompts and outputs in scan results contain descriptions that might be disturbing to some users.

### Basic attack

Focus on default risk categories (violence, sexual, hate & unfairness, self-harm) with a low prompt count to validate the pipeline end-to-end.

Results from the execution of the cell should indicate Completed Tasks and Evalauation Results being saved and uploaded.
![Create Env](../images/basicpycell.png)

Be sure to review the risk_categories avaliable in the array, While basic focuses on Violence and HateUnfairness there are others [documented here](https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/evaluation-evaluators/risk-safety-evaluators)

Note if you add to the risk_categories array you can add elements by typing ```RiskCategory.``` and then seeing other categories to load for testing
![Loading other categories](../images/basicpycellcat.png)

### Intermediate attack

Increase the objectives count and optionally introduce additional attack strategies (e.g., obfuscation, role-play) to probe guardrails more thoroughly.

Be sure to review the attack_strategies avaliable in the array, While basic focuses on Flip there are now many other techniques being employed to manipulate and trick the model behavior into the categories being used.
![Loading other attack strats](../images/expandpycell.png)

Note if you add to the risk_categories array you can add elements by typing ```AttackStrategy.``` and then seeing other strategies that can be employed to load for testing
![Loading other attack strats](../images/expandpycellstrat.png)

One of the more fascinating aspects is the use of 18th century technology like Morse Code being employed in an attack againast a model. Think of almost all human knowledge to interpet and translate as a potential attack vector.

### Advanced and custom attack

Add bespoke high-risk or application-specific prompt objectives and enable more complex PyRIT transformation strategies. Correlate successful attacks with mitigations (system messages, content filters) before progressing to production. 

Be sure to review in the data directory the prompts.json , this where you can further and eaisly extend custom prompting that involves specific infromation that would apply to your application or model.
![Loading custom prompts](../images/custompromptjson.png)

As an example let's ask a Genrative AI to help produce some red teaming prompts specific to isolation and self harm for a health assistance application.
![Loading custom prompts](../images/custompromptinput.png)

## Review AI red team results

1. Within Azure 

## Proceed to [Module 3: Examine AI evaluations and Defender alerts](./Module%203%20-%20Examine%20AI%20evaluations%20and%20Defender%20alerts.md)
