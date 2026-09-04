# SOC Engagement Portfolio: Cloudora

## What this engagement was

This repository documents 20 SOC alert investigations from the Cloudora simulated environment, covering Alerts 13–32 (CLD-0213 through CLD-0232).

The alerts developed into a connected investigation involving phishing, compromised accounts, malicious OAuth consent, MFA attacks, token/session abuse, suspicious inbox rules, data exposure concerns, and persistence. The engagement also included legitimate activity and false positives that had to be separated from the malicious activity through investigation rather than simply trusting the alert.

## How I worked

For each alert, I started with both a malicious and benign hypothesis and worked from the available evidence before deciding on a verdict.

My investigations included:

* Writing KQL queries to investigate and scope activity
* Establishing normal user and host baselines
* Correlating sign-in, proxy, audit, email, and endpoint evidence
* Checking IP addresses, domains, users, devices, timestamps, and related incidents
* Verifying handover and helpdesk context against raw telemetry
* Following attacker activity across related alerts without assuming two events were connected without evidence
* Determining actual impact before assigning severity
* Separating True Positive/False Positive decisions from severity and escalation decisions
* Checking whether containment actually worked and looking for activity that survived remediation
* Documenting evidence gaps and discrepancies instead of filling them with assumptions
* Creating retraceable justifications so another analyst could reproduce my conclusions

## What I found

The engagement showed how individual alerts can become pieces of a much larger incident.

I investigated activity involving a compromised guest account, malicious OAuth consent with persistent permissions, internal payroll-themed phishing, successful token/session abuse, malicious mailbox rules, repeated authentication attempts from known attacker infrastructure, and persistence associated with a scheduled task.

I also encountered alerts that looked serious but were legitimate. These included documented international travel, an approved privileged-group change, and a simulated ransomware artifact from an authorized security-awareness exercise. Those investigations reinforced that detection accuracy does not automatically mean the underlying activity is malicious.
Later alerts demonstrated the importance of investigating beyond initial containment. Known malicious infrastructure continued attempting authentication after controls were deployed, while a surviving scheduled task continued trying to contact malicious Bexley infrastructure. A separate alert revealed that a legitimate pre-reset refresh token remained usable because session revocation had not been applied to every affected account.
One of the biggest lessons was that an alert being blocked or contained does not automatically determine its severity. I had to consider actual impact, targeting, account/data sensitivity, persistence, and whether the activity met the engagement's escalation criteria.

## What I would do differently

The QA feedback helped improve my investigations as the engagement progressed.

Earlier in the engagement, I sometimes placed too much weight on whether an attack had been blocked or contained when determining severity. I learned to look more closely at what had already occurred, what access or persistence remained, how sensitive the affected account or data was, and whether the attacker demonstrated knowledge of Cloudora's current business activity.

I would also separate four questions more clearly from the beginning of every investigation:

1. Did the malicious or policy-violating activity actually occur?
2. What was the actual impact?
3. What severity does that impact justify?
4. Does the activity meet the escalation criteria or provide useful threat intelligence?

Another important lesson was to verify containment instead of assuming that a completed remediation action ended the incident. The later investigations showed that persistence, service accounts, sessions, and tokens can survive incomplete remediation.

Overall, the QA process pushed my investigations from simply deciding whether an alert was "bad" toward understanding the complete attack chain, measuring actual impact, and producing conclusions that another analyst could independently retrace.

## Contents

* `verdicts/` — One file per alert containing my investigation, evidence, KQL queries, verdict, severity, actions, justification, tuning recommendations, and QA result.
* `handovers.md` — My end-of-shift handovers showing how the investigation and incident context developed across shifts.

All data in this engagement is synthetic, including reserved IP ranges and domains. The investigations, queries, verdicts, reasoning, and documentation represent my own work during the engagement.
