# Examine AI evaluations and Defender alerts

> **Avg review time: ~15 min**

This guide provides instructions for examining the AI red teaming agent results across areas relevant to different personas protecting the GenAI application. One of the byproducts of running the AI red team agent is that it can produce security alerts due to it's attack strategy techniques employed on risk category content. Defender for AI Services will generate secuirty alerts that can be found where the persona is.

## Azure AI Foundry project red team evaluations

Security alerting from Defender for AI Services can be found in the Azure AI Foundry Left navigation's blade, here the data analyst or application owner can find if an alert was recently issued and some basic infromation on the alert and affected resource, including some remediation steps. The user is invited to review the alert in more detail and evidence in Defender for Cloud.

![AI Foundry Alert](../images/aifoundalert.png)

![AI Foundry Alert Details](../images/aifoundalert2.png)

## Defender for Cloud

The security alert is also in Defneder for Cloud and in the left navigation Security Alerts. The alert itself contains some mor security context including aspects of the sucpicious prompt that triggered the alert. 

![MDC Alert](../images/mdcalert.png)

![MDC Alert](../images/mdcalertdetails1.png)

Some of those ascpects include:

- Mitre Tactics
- IP address
- Geo information
- model invovled in attack.

![MDC Alert](../images/mdcalertdetails2.png)

The Supporting Evidence and show events in the bottom right provider even more rich data can be found like:

- Suspicious prompt segemnt
- User agent involved with browser or aapplication.
- Confidence score

![MDC Alert](../images/mdcalertdetails3.png)

## Defender XDR alerting

Finally the Defender for AI Services alerting is available in the Defender XDR portal, and can also be correlated with other suspicious or malicious activity around similar patterns. The following below shows a Jailbreak attempt as part of a correlated larger attack story. The same evidence and information is available in differnt tles as well including the same infromation in the Defender for Cloud alert like Prompt Suspicious Segment.

![XDR Alert](../images/xdralert.png)

## AI-SPM within Azure AI Foundry and Defender for Cloud

Security Recommendations are also generated to reduce attack surfaces and harden Azure Services including Azure AI Foundry, these results can be found across areas relevant to different personas protecting the GenAI application. Agin this will include Azure AI Foundry Project -> Guardrails + controls -> Security Recommendations . By clicking on a recomendation you can get some additional details and a button to send you to more infromation found in Defender for Cloud.

![AI recommendation](../images/aispmrec.png)

and Defender for Cloud -> Recomendations

![MDC recommendation](../images/mdcspmrec.png)

Defender for Cloud will have additional context involving the MITRE Tactics involved with the attack surface. Any additional risk factors, and a top suggested active user for assignment for remediation.
