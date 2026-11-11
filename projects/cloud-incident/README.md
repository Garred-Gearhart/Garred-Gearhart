# Cloud and Endpoint Incident Investigation

**Focus:** SOC investigation, cloud security, incident scoping  
**Environment:** TripleTen training exercise using historical Frothly data  
**Report date:** July 1, 2026  
**Tools and evidence:** Splunk, AWS CloudTrail, S3 access logs, endpoint and Microsoft 365 telemetry

## Objective

Investigate suspicious activity across a simulated organization's cloud, email, and endpoint systems, determine whether the activity warranted escalation, and document the scope and response priorities.

## My investigation

I used Splunk to review IAM activity, investigate a public S3 bucket, and follow evidence of exposed credentials. I then reviewed endpoint and email activity to identify additional affected systems and suspicious account changes.

Rather than treating every alert as an isolated event, I organized the findings by affected service, account, and host. The dataset contained multiple types of malicious activity; their presence in the same environment does not, by itself, establish that every event belonged to a single attacker or attack chain.

## Findings and evidence

| Area | Finding documented in my report | Why it mattered |
| --- | --- | --- |
| AWS access | An exposed access key was associated with the `web_admin` profile and a public repository backup file | Required credential revocation and review of activity performed with that identity |
| S3 | A bucket ACL change granted public access; subsequent object uploads were recorded | Required review of both exposure and unauthorized changes |
| Endpoint | BSTOLL-L showed high CPU usage and mining-related activity | Supported investigation of resource abuse |
| Email | An Exchange transport rule named `SOX` and a sensitive-keyword mailbox search were identified | Raised concern about email collection or forwarding; actual data loss still needed scoping |
| C2 | ABUNGST-L and FYODOR-L communicated with a suspected command-and-control path | Supported urgent endpoint investigation and containment |
| Account changes | A new local account was added to administrative groups on FYODOR-L | Indicated a potential means of maintaining elevated access |

I assessed the incident as **High severity / True Positive** in the training report and recommended escalation to Tier 2 or incident response.

## Recommended response

1. Preserve relevant cloud, endpoint, and email evidence.
2. Revoke the exposed cloud credential and review its use.
3. Isolate affected endpoints and investigate suspicious account changes.
4. Review public storage access and remove unintended exposure.
5. Investigate the transport rule, recipients, and mail-flow records before determining the extent of email exposure.

These were recommendations in my report; this investigation did not demonstrate completion of production containment or recovery.

## What I learned

The most useful report connects an observation to its impact and the next question to investigate. I also learned to distinguish a suspicious configuration or connection from proof of data exfiltration.

## Investigation queries

[AWS investigation searches](queries/aws-investigation.spl) contains adapted examples from my original notes. An API event's MFA field is context for investigation, not sufficient evidence of malicious activity by itself.

[Back to portfolio](../../README.md)
