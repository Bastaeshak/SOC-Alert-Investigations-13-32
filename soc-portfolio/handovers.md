## Shift 4 · 2026-08-29

Cloudora SOC - Shift 4 Handover Brief
Incidents / Alerts
CLD-0213 - OAuth Consent - Escalated - Medium
Previously compromised guest account ewalsh.ext@cloudora.io authorized the first-seen, unverified Bexley Sync Utility.
Requested Mail.Read, Files.Read.All, and offline_access.
Proxy evidence confirmed the OAuth consent POST from Eleanor's session.
Available telemetry could not establish what resources the application accessed, meaning the activity could not be fully bounded.
Escalated to vCISO.
Revoke application consent/tokens, remove the application, and continue investigation of Eleanor's mailbox and file activity.
Historical review recommended for the Bexley phishing infrastructure at 192.0.2.203 to identify other recipients or users who interacted with the phishing link.
Note evidence discrepancy: alert narrative and raw proxy telemetry contain different dates for the consent event and should be reconciled.
CLD-0214 - Payroll Data Sent to Personal Email - Insufficient Data - Medium
marcus.oje@cloudora.io sent payroll_reconciliation_sept.csv, containing 2,140 rows with salary fields, to moje.home@personal-mail.example.
Sign-in immediately preceding the transfer was Marcus's usual London/registered-device session with MFA satisfied.
No helpdesk or shift-brief authorization was identified for sending payroll information to a personal mailbox.
Contact Marcus and his manager to establish business justification and authorization.
If unauthorized or malicious, escalate immediately under the confirmed payroll-data access/exfiltration trigger.
Recommend strengthening DLP controls to block or require approval for payroll data sent to personal/external email addresses.
CLD-0215 - Ransom Note Filename - False Positive - Informational
tara.kemp@cloudora.io created READ_ME_RESTORE.txt on LDN-SRV-FS1.
AV telemetry showed no-encryption-activity=TRUE, with no file storm or associated malicious process activity.
Helpdesk ticket HD-5150 documented a vCISO-approved October awareness exercise involving a simulated ransom-note file and EICAR placement.
Closed as legitimate awareness activity.
No containment required.
Repeated MFA Denials - True Positive - Low
Five MFA push denials targeted ops-admin@cloudora.io from hosting-provider IP 192.0.2.88.
Password evaluation succeeded before MFA, confirming the valid ops-admin password was known and should be considered burned.
MFA prevented successful authentication.
KQL found no successful sign-in from 192.0.2.88, no other accounts targeted by the IP, and no follow-on activity in available telemetry.
Pattern directly matches the Shift 4 intel warning regarding MFA fatigue against admin/operations accounts from hosting infrastructure with valid credentials already obtained.
Reset the ops-admin password immediately, revoke active sessions/refresh tokens, invalidate existing authentication cookies, and require fresh authentication.
Block 192.0.2.88 and add it to Cloudora's malicious IOC/blocklist.
Review recent ops-admin authentication and privileged activity for evidence of earlier successful use of the compromised credential.
Flag to vCISO as relevant threat intelligence despite not meeting the formal escalation threshold.
Shift 4 IOC / Watchlist Updates
192.0.2.88 - BLOCK - Source of confirmed MFA-fatigue activity against ops-admin; valid password successfully passed authentication's password stage.
192.0.2.146 - Continue watchlist - associated with previously escalated suspicious ewalsh.ext activity.
192.0.2.203 - Continue watchlist - Bexley phishing infrastructure. Recommend historical recipient/click review to determine whether additional users interacted with the phishing campaign.
Items for Next Shift
Continue investigation and containment of Bexley Sync Utility and the previously compromised Eleanor Walsh guest account.
Review historical mail/proxy telemetry for other recipients and interactions involving the 192.0.2.203 Bexley phishing infrastructure.
Follow up with Marcus Oje and his manager regarding the payroll export to his personal mailbox.
Treat unauthorized payroll transfer confirmation as an immediate escalation trigger.
Confirm ops-admin password reset, session/token revocation, authentication-cookie invalidation, and blocking of 192.0.2.88.
Continue watching for MFA-fatigue attempts against other admin/operations accounts or attempts against ops-admin from different infrastructure.
No action required for the approved HD-5150 awareness artifacts unless activity deviates from the documented exercise.

## Shift 5 · 2026-08-30

END-OF-SHIFT HANDOVER - WEEK 1, SHIFT 5

CLD-0217 - Off-hours upload to young domain
Verdict: Escalate
Severity: High

LDN-SRV-APP1, the payroll integration middleware server, made 214 POST requests to bexley-sync.example (192.0.2.150) between 02:41 and 03:20 UTC, transferring approximately 41 MB.

Compared the activity against the documented Bexley feed. Legitimate activity uses SFTP to transfer.bexleypay.example, and the expected SFTP transfer was separately observed at 03:00 UTC. The suspicious destination was registered six days ago and matches current intelligence regarding recently registered supplier-lookalike staging infrastructure.

IOC scoping found no other Cloudora hosts communicating with bexley-sync.example or 192.0.2.150 in the available proxy telemetry.

The contents of the 41 MB transfer remain unknown. Escalated because the suspicious outbound transfer is confirmed and the full impact/content of the transfer cannot be bounded with the available evidence.

Pending:
- Determine what data was contained in the 41 MB transfer.
- Investigate the process/service responsible for the POST activity on LDN-SRV-APP1.
- Preserve evidence and contain LDN-SRV-APP1 as directed by the vCISO.
- Block bexley-sync.example / 192.0.2.150 as appropriate.
- Continue scoping for related infrastructure or persistence.


CLD-0218 - Sign-in from new country
Verdict: False Positive
Severity: Informational

omar.farah@cloudora.io successfully signed in from Dubai at 06:40 UTC from 198.51.100.201 using his registered Windows laptop with MFA satisfied.

The Shift 5 brief states Omar is on his regular monthly Dubai client visit under HD-5177 and the travel calendar. Raw sign-in telemetry also showed an established monthly Dubai pattern, with previous activity noted on 12 Sep, 14 Aug and 11 Jul.

Closed as legitimate documented travel activity.

No pending action.


CLD-0219 - Privileged group change
Verdict: False Positive
Severity: Informational

tara.kemp was added to Payroll-Admins.

HD-5170, dated 2026-10-09 08:45 UTC, documents:
"Role change: tara.kemp to Payroll-Admins per integration RACI; approved change HD ref CHG-2105"

The helpdesk record matches the user and privileged group involved in the alert and documents the change as approved.

Closed as authorized administrative activity.

No pending action.


MID-SHIFT ALERT - Inbound volume anomaly
Verdict: True Positive
Severity: Medium

finance@cloudora.io received 380 unsolicited newsletter and account-confirmation messages between 07:00 and 07:30 UTC.

Reviewed email evidence confirmed that the mailbox had not subscribed to the services. The sample message passed SPF, DKIM and DMARC, consistent with legitimate services being abused to generate a subscription bomb.

The activity matches the Shift 5 intelligence regarding subscription bombing being used as a smokescreen to potentially bury a payment-change, invoice, supplier, or other finance-related message.

Available KQL and email evidence did not identify the specific malicious or fraudulent message the subscription bomb may have been intended to conceal. Historical August activity returned during searching was not technically connected to the current incident and was excluded.

No formal escalation trigger was confirmed. The activity should still be flagged to the vCISO as useful threat intelligence because the finance mailbox was deliberately targeted.

Pending:
- Treat the subscription bomb as possible cover for a malicious finance-related email buried within the flood.
- Continue looking for unusual invoice, payment-change, supplier, bank-detail, account-security, or other business-sensitive messages received around 07:00-07:30 UTC, expanding the window as needed.
- The specific hidden message has NOT been identified. Do not assume what it was without evidence.
- Review message trace/mail-flow data to identify messages received during the flood that differ from the subscription confirmations.
- Finance should carefully review business-sensitive messages received during and around the flood before acting on payment or supplier changes.
- Continue monitoring for follow-on fraud or account activity.


SHIFT 5 PRIORITIES FOR NEXT ANALYST

1. CLD-0217 remains the priority investigation. The 41 MB outbound transfer from the payroll integration server is confirmed, but the contents and full impact remain unknown.

2. Continue investigating the finance subscription bomb. Be specifically alert for a potentially malicious finance-related email hidden within the 380-message flood. The flood is confirmed, but the specific message it may have been intended to conceal has not yet been found.

3. Review mail-flow/message-trace evidence around the subscription bomb for invoice, payment-change, supplier, bank-detail, or account-change activity.

4. Keep CLD-0217 and the subscription-bomb investigation separate unless technical evidence establishes a connection.

5. Do not connect historical August incidents or other Bexley activity to current events without supporting technical evidence.

## Shift 6 · 2026-08-31

Cloudora - Shift 6 Handover
CLD-0221 - Suspicious Inbox Rule
Verdict: Escalate
Severity: High
Daniel Reeve's mailbox created a hidden inbox rule from known malicious IP 192.0.2.146.
Rule forwarded messages containing invoice, payment, or Bexley to an external address and marked them as read.
Confirms successful mailbox compromise and attacker persistence/collection behavior.
Escalated to vCISO.
Next: Keep open, revoke sessions/tokens, remove the rule, and determine what emails were forwarded or accessed.
CLD-0222 - Internal Payroll Phishing
Verdict: Escalate
Severity: High
Compromised ewalsh.ext account sent an authenticated internal payroll/Bexley phishing email to 12 Finance/HR users.
Email was sent from 192.0.2.146 and linked to 192.0.2.203. Both are known malicious/watchlist IPs.
Daniel clicked the phishing link about 22 minutes later.
Daniel later showed suspicious token authentication and mailbox manipulation.
Separate suspicious Marcus Oje activity was found from 192.0.2.146, but it predates this phishing email and should not be assumed related without further evidence.
Escalated to vCISO.
Next: Investigate all recipients, contain affected accounts, and determine whether sensitive data was accessed.
CLD-0223 - Daniel Token Replay
Verdict: Escalate
Severity: High
After Daniel clicked the phishing link, his account later had successful token authentication from 192.0.2.146.
MFA showed claim satisfied with no prompt.
Six minutes later, Daniel's OWA session created the malicious CLD-0221 inbox rule.
Strong evidence of token/session replay followed by mailbox compromise.
Escalated to vCISO.
Next: Revoke Daniel's sessions/tokens, review mailbox activity during the compromise window, and continue IOC scoping.
CLD-0224 - AV Scheduled Digest
Verdict: True Positive
Severity: Low
MAN-WS-204 BundleLoader was quarantined successfully and rescanned clean.
LDN-WS-133 PopServe adware was quarantined successfully but later appeared again and was quarantined again.
LDN-WS-140 EICAR was confirmed as an authorized security-awareness exercise.
No failed remediation or broader compromise found.
Discrepancy: Digest says 3 detections, but raw AV logs show 4 detection events across 3 hosts because PopServe occurred twice.
Closed as remediated.
Next: Monitor LDN-WS-133 for another PopServe detection.
Major Shift 6 Finding

CLD-0222, CLD-0223, and CLD-0221 appear to be part of the same attack chain.

The compromised Bexley account sent internal phishing. Daniel clicked the malicious portal. His account later showed successful token authentication from attacker infrastructure without an MFA prompt. His OWA session was then used to create a malicious forwarding rule.

Priority for Next Shift - Conditional Access / IOC Blocking
Known malicious IPs 192.0.2.146 and 192.0.2.203 continue appearing in the investigation, including successful or active-session activity.
We need to determine why known malicious infrastructure was still able to successfully access accounts or sessions despite the expected protections.
Verify exactly when the IOC blocks and Conditional Access controls became active.
Determine whether the controls cover all authentication and session types.
Determine whether stolen/replayed tokens or existing sessions can continue working despite the IP blocks.
Check for a Conditional Access policy gap, configuration issue, timing gap, or incomplete IOC enforcement.
Do not assume Conditional Access failed until the control timeline and behavior are verified.
High priority: If malicious access succeeded after the controls were confirmed active, notify the vCISO immediately and investigate the root cause because containment may not be reliable.

## Shift 7 · 2026-09-02

SHIFT HANDOVER – CLOUDORA

- Bexley incident remains active. Continue treating 192.0.2.146, 192.0.2.203, 192.0.2.150, 192.0.2.88 and associated Bexley domains as incident IOCs.

- CLD-0226: Confirmed malicious BexleySyncHealth persistence activity on LDN-SRV-APP1. Task-associated POST attempts to bexley-sync.example / 192.0.2.150 occurred at 02:58 and 06:58 UTC Oct 13 and were blocked at proxy. Only APP1 was observed contacting this domain in available proxy telemetry. APP1 was documented as seized/offline Oct 12, but telemetry continued Oct 13. Follow up on why containment did not match the documented timeline. Isolate APP1, preserve endpoint evidence, investigate task creation/execution, and validate APP2 as clean before completing payroll migration.

- CLD-0225: Continued sign-in attempts from known malicious IPs 192.0.2.146 and 192.0.2.203 targeted daniel.reeve, hr-manager, ewalsh.ext, and marcus.oje. All attempts were blocked by Conditional Access before password evaluation. No successful authentication or follow-on audit activity identified. Existing incident blocks are holding. Continue monitoring and flag continued attempts as threat intelligence.

- Genuine Bexley SOW email: hr-manager reported an Oct 13 07:55 UTC email from e.walsh@bexleypay.example. Message followed normal Bexley mail path, passed SPF/DKIM/DMARC, and contained a clean PDF with no active content. Assessed FP/Informational. However, available audit/helpdesk evidence does not confirm Eleanor's compromised account was reset/session-revoked. Verify containment with Eleanor/Bexley through a trusted channel.

- Identity alert volume ran 3.1x the four-week average. Increase appears consistent with genuine malicious identity activity during the Bexley incident rather than demonstrated rule sensitivity. No broad threshold tuning recommended. Review guest/executive rules individually for duplicates or repeated false positives and reassess after incident activity decreases.

PRIORITY FOR NEXT SHIFT:
1. Verify APP1 is actually isolated and determine why activity continued after the documented Oct 12 seizure.
2. Obtain endpoint/EDR and Task Scheduler evidence for BexleySyncHealth and scope persistence across other endpoints.
3. Validate APP2 is clean before restoring the legitimate Bexley SFTP feed.
4. Confirm Eleanor's account reset/session revocation with Bexley.
5. Continue monitoring known Bexley IOCs and verify existing Conditional Access/proxy blocks continue to hold.

## Shift 8 · 2026-09-04

CLD-0229 — Bexley Supplier Breach Notification
Verdict: True Positive
Severity: Medium
Completed the Bexley IOC sweep. Prior malicious infrastructure matched known Cloudora activity. A new hit was found from LDN-WS-118, Chloe Iyer’s finance workstation, to the newly disclosed malicious domain bexley-docs.example. Only one request was found with no evidence of account compromise, malware execution, persistence, or payroll/client-data access. Bexley only provided partial hashes, so a complete hash sweep could not be performed.

CLD-0230 — Large Transfer from LDN-SRV-APP2
Verdict: False Positive
Severity: Informational
The 96 MB SFTP transfer to transfer.bexleypay.example was confirmed as the expected first-run payroll backlog transfer under CHG-2110 / HD-5205. Destination, protocol, and later scheduled traffic matched normal payroll activity. No containment required.

CLD-0231 — Pre-Reset Refresh Token Still Active
Verdict: False Positive
Severity: Informational
Marcus Oje’s token exchange came from his normal London office IP and registered Windows device, so the current activity was legitimate. However, the alert exposed a remediation gap: Marcus’s old refresh token remained valid because Monday’s session revocation was only completed for Daniel Reeve and Eleanor Walsh.

CLD-0232 — Marcus Oje Malicious Inbox Rule
Verdict: True Positive
Severity: Medium
Confirmed Marcus’s account was compromised. At 23:57 UTC on October 8, known attacker IP 192.0.2.146 created a hidden inbox rule that moved messages containing invoice/remittance to RSS Subscriptions and marked them read. Marcus later reported missing invoice emails. The same attacker IP created a similar rule in Daniel Reeve’s mailbox. Marcus was missed during Monday’s mailbox remediation, allowing the malicious rule to remain.

Key Shift Finding

Monday’s remediation was too narrowly scoped. Marcus had both a surviving pre-reset token and a malicious inbox rule that was not removed. Remediation needs to cover the full affected-account scope, not only the accounts that originally alerted.

Next Shift Watch

Verify that Marcus’s sessions/tokens are revoked and the malicious inbox rule is removed. Check the rest of Monday’s reset list for surviving sessions or mailbox persistence. Continue watching Bexley IOCs, especially 192.0.2.146, 192.0.2.203, 192.0.2.150, 192.0.2.88, and bexley-docs.example. Follow up on Chloe Iyer / LDN-WS-118 if additional browser, email, or endpoint evidence becomes available. Rerun the Bexley hash sweep if full SHA256 hashes are provided, and watch for additional hidden invoice/remittance rules or other evidence that previous remediation missed affected accounts.
