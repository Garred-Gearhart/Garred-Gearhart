# Web Application Remediation and Validation

**Focus:** Defensive controls and evidence-based validation  
**Environment:** MegaQuagga TripleTen lab  
**Report date:** March 4, 2026  
**Tools:** pfSense, reverse proxy, ModSecurity, Graylog, Wireshark

## Objective

Reduce direct exposure of a vulnerable WordPress server, enforce web request inspection, and validate the changes using traffic and logs.

## Changes I implemented

- Placed a reverse proxy in front of the WordPress application.
- Configured ModSecurity to inspect requests and block matching attacks.
- Changed the WAF from `SecRuleEngine DetectionOnly` to `SecRuleEngine On`.
- Configured the external HTTPS path and HTTP-to-HTTPS redirection.
- Updated pfSense NAT and firewall rules to support the intended traffic path.
- Configured centralized logging in Graylog and used Wireshark to inspect traffic.

## Validation recorded in my report

| Check | Before | After | Evidence reference in the original report |
| --- | --- | --- | --- |
| Request path | The test workstation could reach WordPress directly | Traffic was routed through the reverse proxy | `pfsense-LAN-2.pcap`, packets 4 and 11 |
| Malicious request | A tested file-inclusion request reached the application and returned sensitive file content | The tested request received `403 Forbidden` | `pfSense-LAN-3.pcap`, packet 42 |
| WAF enforcement | Detection-only mode recorded suspicious requests | Active enforcement blocked the tested request | ModSecurity rule-engine setting and observed response |
| External HTTP access | The website accepted unencrypted HTTP | An HTTP request received an HTTPS redirect | `pfSense-LAN-4.pcap`, packet 246 |
| Backend traffic | The application was directly exposed in the original test path | Internal proxy-to-application HTTP remained visible | `pfSense-LAN-4.pcap`, packet 270 |

The packet references above come from my original report. Raw PCAPs and screenshots are not bundled in this portfolio, and the tests have not been rerun during preparation of these write-ups.

## What the results establish

The documented test showed a changed traffic path, enforcement against the tested malicious request, and redirection of external HTTP traffic to HTTPS.

HTTPS protects the encrypted connection; it does not fix an application vulnerability. A WAF adds a mitigating control, but it does not replace patching the application. Because the report showed internal HTTP between the proxy and application, I do not claim end-to-end TLS encryption.

Similarly, a successful block of one test request does not prove protection against every variant. Additional testing should check legitimate traffic, bypass paths, rule coverage, certificate validation, and repeated attacks after changes.

## Further improvements I recommended

I recommended recurring vulnerability scanning and patch management, stronger administrative authentication, and additional network monitoring. These recommendations are distinct from the controls implemented in this exercise.

## What I learned

Configuration changes need observable validation. The useful evidence was not simply that a WAF was installed, but that enforcement was enabled and the tested request was blocked while the traffic followed the intended path.

[Assessment and penetration test](../vulnerability-assessment/README.md) · [Back to portfolio](../../README.md)
