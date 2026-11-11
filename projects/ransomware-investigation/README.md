# Web Compromise and Ransomware Investigation

**Focus:** Incident investigation, scope, and business impact  
**Environment:** TripleTen training exercise using historical security events  
**Report date:** June 9, 2026  
**Tools and evidence:** Splunk, HTTP events, Suricata alerts, Windows process and registry telemetry

## Objective

Investigate suspicious web activity and a Cerber ransomware infection, identify the systems involved, and prepare a report that could support incident response escalation.

## My investigation

I reviewed scanning and authentication activity against a Joomla website, examined suspicious files and associated infrastructure, and investigated ransomware activity involving a workstation and a connected file server.

I used multiple event sources to connect technical artifacts to affected assets. I kept web-compromise findings and ransomware findings distinct where the evidence did not establish a direct causal link between them.

## Findings and evidence

| Investigation area | Finding recorded in my notes and report |
| --- | --- |
| Web reconnaissance | HTTP activity included an Acunetix scanner user-agent |
| Authentication attack | More than 400 password attempts targeted the Joomla server; my report documented subsequent administrator access |
| File artifact | `3791.exe` appeared in the Joomla web root; a file hash was recorded for investigation |
| Ransomware alert | A Suricata Cerber check-in signature supported review of the workstation's activity |
| Process activity | A VBScript and `121214.tmp` were associated with the infection investigation |
| Payload artifact | My report identified `mhtr.jpg` as containing a ransomware payload |
| Scope | The report recorded 257 unique PDF files affected on a remote file server and 401 text files affected in a user profile |

The training report assessed the incident as **Critical / True Positive** because it documented ransomware-related loss of file availability.

## Evidence limits

The file-impact totals above are the conclusions recorded in my original report, not newly reproduced measurements. The counting searches in my notes identify file activity and distinct filenames; those counts alone do not prove encryption. A complete validation would correlate the relevant operations, process activity, timestamps, and changes to the files.

Likewise, a repeated password in HTTP requests does not independently establish a successful login. Authentication outcomes or authenticated activity are needed to support that conclusion.

## Recommended response

I recommended isolating affected systems, preserving evidence, reviewing accessible file shares, resetting compromised credentials, and blocking confirmed malicious indicators. Recovery should use validated clean backups after the incident's scope and persistence mechanisms are understood.

These are response recommendations. I did not perform production ransomware recovery as part of this exercise.

## What I learned

Scoping an incident means looking beyond the initially infected host. Accessible shared storage can expand the impact, and a clear report should explain what is confirmed, what is inferred, and what still needs validation.

[Example investigation searches](queries/ransomware-investigation.spl) · [Back to portfolio](../../README.md)
