# WordPress Brute-Force Alert Triage

**Focus:** Alert validation, authentication analysis, escalation  
**Environment:** TripleTen training lab  
**Report date:** May 27, 2026  
**Evidence:** ModSecurity alert, packet captures, HTTP authentication traffic

## Objective

Determine whether an alert represented a real authentication attack, identify affected accounts, and document a clear severity and escalation decision.

## My investigation

I analyzed traffic targeting a WordPress login portal and reconstructed the sequence of authentication attempts. The traffic included a Hydra user-agent and repeated requests to `wp-login.php` from one lab host.

I reviewed the packet captures for the targeted accounts, request contents, and the authentication outcomes documented in my report. The investigation found exposure of valid credentials for both a standard user and an administrator account.

## Reconstructed sequence

The following times are from the historical lab traffic, **April 10, 2025 (UTC)**. They are event times, not the date I completed the report.

| Time | Activity documented in my report |
| --- | --- |
| 15:42:22 | Repeated authentication attempts began |
| 15:44 | Successful authentication for the standard account was recorded |
| 15:48 | Successful authentication for the administrator account was recorded |
| 15:48:41 | The observed attack activity ended |

My report measured the attack window at approximately **6 minutes, 19 seconds** and assessed the alert as **High severity / True Positive — Credential Compromise**.

## Why I escalated it

The key issue was the documented compromise of an administrative account, rather than just a high number of failed requests. Administrative access could allow application changes, malicious uploads, or access to stored information. These were potential follow-on impacts; I did not treat them as actions proven to have occurred.

## Recommended actions

- Reset compromised credentials and invalidate active sessions.
- Review administrative activity, plugins, and files for unauthorized changes.
- Investigate the targeted host for further compromise.
- Require MFA for administrators and apply controls against repeated login attempts.
- Protect authentication traffic with TLS and monitor for suspicious login patterns.

## What I learned

Attack tooling and request volume help identify suspicious activity, but the escalation decision needs evidence of outcome and impact. I also learned why exposed passwords should be omitted from public reports, even when the source material comes from a lab.

**Publication note:** Passwords and raw credential-bearing traffic have been excluded. The timeline is summarized from my original report; raw captures are not included in this repository.

[Back to portfolio](../../README.md)
