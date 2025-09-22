# Run AI red team evaluations agent

> **Avg execution time (section): ~25 min**

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

The Basic attack focuses on default risk categories (violence, sexual, hate & unfairness, self-harm) with a low prompt count to validate the pipeline end-to-end.

Results from the execution of the cell should indicate Completed Tasks and Evaluation Results being saved and uploaded.

![Create Env](../images/basicpycell.png)

Be sure to review the risk_categories available in the array. While Basic focuses on Violence and Hateful/Unfair Content, there are others [documented here](https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/evaluation-evaluators/risk-safety-evaluators).

If you want to add to the risk_categories array, you can add elements by typing ```RiskCategory.``` and choosing other categories from the IntelliSense suggestions list.

![Loading other categories](../images/basicpycellcat.png)

### Intermediate attack

The Intermediate attack increases the objectives count and optionally introduces additional attack strategies (e.g., obfuscation, role-play) to probe guardrails more thoroughly.

Be sure to review the attack_strategies available in the array. While Basic only used the Flip strategy, Intermediate employs many other techniques to manipulate and trick the model behavior into the categories being used.

![Loading other attack strats](../images/expandpycell.png)

Just like the risk_categories array in the Basic attack, if you want to add additional attack strategies to the Intermediate attack, you can add elements to the attack_strategies array by typing ```AttackStrategy.``` and choosing other strategy types from the IntelliSense suggestions list.

![Loading other attack strats](../images/expandpycellstrat.png)

One of the more fascinating strategies is the use of early 19th century technology like Morse Code being employed in an attack against an AI model. Almost all human knowledge can be used to interpret and translate prompts as a potential attack vector!

### Advanced and custom attack

The Advanced attack delivers bespoke high-risk or application-specific prompt objectives and enables more complex PyRIT transformation strategies. Here, we can correlate successful attacks with mitigations (system messages, content filters) before progressing to production. 

Be sure to review the prompts.json file in the data directory.  This file includes easily extensible custom prompting that would apply to your application or model.
![Loading custom prompts](../images/custompromptjson.png)

As an example, let's ask a Generative AI to help produce some red teaming prompts specific to isolation and self-harm for a health assistance application.

![Loading custom prompts](../images/custompromptinput.png)

## Review AI red team results

> [!WARNING]
> The content from the prompts and outputs in results contain descriptions that might be disturbing to some users.

1. Go to the Azure AI Foundry project resource and launch Azure AI Studio.

![Launch AI Foundry Project](../images/aiprojlaunch.png)

2. In the left navigation, go to the Evaluation and the AI red teaming tab.

![Goto AI Evaluations](../images/redteameval.png)

3. Let's examine the Advanced Scan where we see some percentages above 0% indicating some successful attacks.  Click the Advanced-Scan- name.
4. Within the report we have high level attack success in some risk categories. Be sure to note these in production. Operationally, these risk categories and attack strategies can then be used to benchmark and track progress to the application to determine how content filters and data sources are further securing the models.

![Adv attack report](../images/redteamevaladv.png)

5. Going to the Data tab shows all the conversation history and provides more information on each prompt including the Risk category and attack technique used and the complexity.
6. Scroll to the bottom, switch view to 100 results per page, and scroll back up to see some **Attack successful** results. Choose one and click on "view more". In this case, we'll examine a successful Violence attack.

![Adv attack report](../images/viladv.png)

7. You will now see the prompt that was crafted using the Red Team Eval library and Pyrit using attack techniques to successfully bypass content filtering on this model.

![Adv attack report](../images/viladvprev.png)

## Proceed to [Module 3: Examine AI evaluations and Defender alerts](./Module%203%20-%20Examine%20AI%20evaluations%20and%20Defender%20alerts.md)
