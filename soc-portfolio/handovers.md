# End-of-shift handovers · Cloudora

## Shift 1 · 2026-08-25

SHIFT HANDOVER - 08/25/2026

SHIFT STATUS
Alerts investigated: 3
Resolved: 2
On Hold: 1
Escalated: 0

--------------------------------------------------

1. PUA REMEDIATED - MAN-WS-204 / helen.dray

Verdict: True Positive - Remediated
Severity: Low
State: Resolved

PUA:Win32/BundleLoader detected in freepdf_setup.exe.

17:41:52 UTC - Installer downloaded.
05:31:02 UTC - PUA detected.
05:31:04 UTC - Quarantine successful on attempt 1.
07:31:04 UTC - Rescan CLEAN with no reappearance.

No evidence of persistence or continued impact. No escalation required.

User education recommended regarding free/bundled installers and use of approved software.

Tuning: None recommended. Detection behaved as intended.

--------------------------------------------------

2. GUEST ACCOUNT SIGN-IN - ewalsh.ext@cloudora.io

Verdict: Insufficient Data
Severity: Medium
State: On Hold / Monitoring
Escalation: Not currently escalated

Original 21:47 UTC sign-in from 198.51.100.77 matched Eleanor Walsh's established London, macOS/Safari activity.

Morning handover confirms Eleanor is an authorized Bexley consultant working during go-live week. Lack of MFA is expected under the documented Bexley guest-access policy.

During investigation, a separate anomalous sign-in was identified:

IP: 192.0.2.146
Device: Windows
Browser: Chrome
Network: Unknown / hosting infrastructure

This does not match Eleanor's established sign-in pattern. Search of available sign-in logs found no other activity from 192.0.2.146.

Additional proxy/egress review identified:

Oct. 7 11:02 UTC - ewalsh.ext session accessed 34 documents in the Payroll-Mapping SharePoint library over approximately 25 minutes.

Oct. 8 23:52 UTC - ewalsh.ext session performed OAuth consent for "Bexley Sync Utility."

Proxy logs do not provide the originating source IP, so these sessions cannot currently be tied to 192.0.2.146.

NEXT SHIFT ACTIONS:

- Confirm with Eleanor/Bexley whether the 192.0.2.146 Windows/Chrome session was authorized.
- Obtain full session/authentication details for 192.0.2.146.
- Correlate the anomalous session with SharePoint and OAuth activity.
- Determine whether client/payroll data was viewed, downloaded, modified, deleted, or shared.
- Continue monitoring for 192.0.2.146, unfamiliar IPs/devices, unusual SharePoint activity, large downloads, and OAuth consent activity.

ESCALATE TO vCISO IF:

- Eleanor denies the session.
- Unauthorized client/payroll data access is confirmed.
- Confirmed malicious activity expands beyond the current scope.
- Activity cannot be bounded with available evidence.

DO NOT CLOSE - follow-up required.

--------------------------------------------------

3. TCP PORT SWEEP - LDN-SCAN-01 / 203.0.113.44

Verdict: False Positive - Authorized Activity
Severity: Informational
State: Resolved
Resolution: Resolved by Change

LDN-SCAN-01 probed 3,800+ TCP ports across approximately 160 internal hosts using sequential SYN probes with no data transfer.

Normal scan window:
Monday 22:00 - Tuesday 03:00 UTC

CHG-2101 temporary window:
Sunday 23:00 - Monday 03:00 UTC

Observed activity:
Sunday 23:30 - Monday approximately 01:1x UTC

Source, traffic pattern, and timestamps matched the authorized vulnerability scan and approved CHG-2101 window.

No escalation or additional investigation required.

Tuning recommendation: Consider change-window-aware suppression/severity reduction only when the authorized scanner identity AND an active approved scan window match. Continue alerting outside approved windows or for unexpected sources.

--------------------------------------------------

NEXT SHIFT PRIORITY

Priority: ewalsh.ext@cloudora.io / Eleanor Walsh

PUA and TCP port-sweep investigations are complete.

Eleanor investigation remains open pending validation of the 192.0.2.146 session and correlation with subsequent SharePoint/OAuth activity.

## Shift 2 · 2026-08-26

SHIFT 2 HANDOVER

CLD-0205 - Confirmed credential-harvesting phishing email targeting Marcus Oje. One recipient tenant-wide. Marcus reported no credential submission and no downstream compromise identified. docusign-verify.example and 192.0.2.203 documented as malicious indicators. Recommend blocking the domain, IP, and malicious URL at appropriate controls. CLD-0205 remains the incident of record.

CLD-0206 - Atypical travel for Helen Dray determined to be legitimate. HD-5102 documented Lisbon travel Oct 6-10 with expected mobile-only access. Lisbon sign-in matched the documented dates/location, used Helen's registered iOS device, and satisfied MFA.

CLD-0207 - Outbound marketing send determined to be authorized. 640-message October newsletter matched HD-5138, including expected Tuesday timing, mailflow-platform.example connector, and approximately 640 recipients.

CLD-0208 - Duplicate of CLD-0205. Post-delivery sandbox detonation independently confirmed 192.0.2.203 as credential harvesting and established one recipient tenant-wide. New evidence incorporated into CLD-0205.

WATCH NEXT SHIFT:
- Monitor for additional activity involving docusign-verify.example or 192.0.2.203.
- Re-raise CLD-0205 if another user/host is affected, credential compromise is identified, or client/payroll data access is discovered.
- Bexley integration remains active, so continue treating related phishing lures seriously while verifying them against evidence.
- CLD-0205 and CLD-0208 were not present in the available ServiceNow instance, so their ServiceNow cross-reference/duplicate closure could not be completed.

## Shift 3 · 2026-08-27

CLOUDORA SOC - SHIFT 3 HANDOVER

1. PRIORITY THREAT INTELLIGENCE - CLD-0001 IOC ACTIVE AGAIN

Known CLD-0001 IOC 198.51.100.212 was observed attempting to authenticate to daniel.reeve@cloudora.io between approximately 04:40-05:01 UTC.

- Raw sign-in evidence independently confirmed attempts at 04:40:55 and 05:01:33 UTC.
- Both were blocked by the standing Conditional Access policy BEFORE password evaluation.
- No successful authentication or account access was identified.
- Because authentication never reached password evaluation, we cannot determine whether the actor possessed Daniel's valid password.
- Database-wide IOC search found no evidence of 198.51.100.212 targeting additional Cloudora accounts or producing follow-on activity in the available telemetry.
- Alert stated FOUR attempts, but only TWO unique attempts were independently verified in the available signin dataset. Preserve this discrepancy.
- Verdict: True Positive
- Severity: Low
- No additional containment required at this time because the standing block worked.
- Keep 198.51.100.212 blocked.
- Flag renewed CLD-0001 infrastructure activity to the vCISO as threat intelligence.
- Re-raise/escalate if this IOC or related infrastructure successfully authenticates, bypasses Conditional Access, targets additional accounts, or produces follow-on activity.

TUNING:
Do NOT suppress the watchlist rule. It behaved as intended and identified renewed activity from known malicious infrastructure. Repeated blocked attempts remain useful threat intelligence.


2. ELEANOR WALSH / BEXLEY GUEST ACCOUNT

ewalsh.ext@cloudora.io successfully authenticated at 10:55 UTC from previously unseen hosting IP 192.0.2.146 using Windows/Chrome with no MFA challenge.

This significantly differed from the observed baseline of 198.51.100.77 using macOS/Safari.

Following authentication, the session accessed the Bexley-Integration SharePoint site and opened 34 documents in the Payroll-Mapping library within approximately 25 minutes.

- Database-wide search found no other observed activity from 192.0.2.146.
- Helpdesk records provided no explanation for the device/network/activity change.
- Previous shift identified Bexley-themed credential harvesting, but there is NO confirmed technical connection between previous IP 192.0.2.203 and current IP 192.0.2.146.
- Account compromise is probable but NOT conclusively confirmed.
- Verdict: Escalate per SOP because the activity cannot currently be bounded with available evidence.
- Contact Eleanor through a trusted channel to verify the 10:55 UTC Windows/Chrome sign-in and 34-document access.
- Contain/revoke the suspicious session while activity is verified.
- Review detailed SharePoint audit records to identify exactly which 34 documents were accessed, what actions occurred, and whether sensitive client/payroll information was involved.

TUNING:
Do not suppress the unfamiliar guest sign-in rule. Consider enriching it with previous device/IP baseline, MFA status, and post-authentication SharePoint activity. Guest accounts without MFA remain a security concern.


3. SVC-BACKUP LOCKOUT / CHG-2102

svc-backup@cloudora.io experienced repeated authentication failures from 203.0.113.20 / LDN-SRV-BK1 during the documented CHG-2102 backup migration and agent reconfiguration window.

- Failure 50126 observed at 01:55:12 UTC.
- Lockout recorded at 02:11:48 UTC following the 12th failure.
- Source, account, timing, and Service/Legacy authentication matched CHG-2102.
- No unrelated activity from 203.0.113.20 was identified.
- No historical svc-backup records were available to establish a prior baseline.
- Verdict: False Positive
- Severity: Informational
- Operational impact still exists because the service account actually locked out.
- IT Operations / backup infrastructure owner should confirm the account was restored and scheduled backups completed successfully.

TUNING:
Do not fully suppress service-account lockouts during change windows. Consider enriching alerts with approved change information or narrowly reducing priority when the account, source, and approved maintenance window all match.


4. MAN-WS-211 BEACONING / ENDPOINT AGENT PILOT

Beaconing heuristic triggered on MAN-WS-211, assigned to priya.nair, communicating with cdn-metrics.example / 198.18.140.9 every 60 seconds.

Investigation confirmed:
- POST payloads were approximately 412-414 bytes.
- Earliest proxy record explicitly states "agent heartbeat 60s interval begins pilot group."
- HD-5144 documents the Manchester endpoint monitoring pilot and the same cdn-metrics.example 60-second reporting interval.
- Database-wide search of MAN-WS-211 found only the expected proxy heartbeat records.
- No unusually large outbound transfers or unexpected destinations were identified in the available proxy telemetry.
- Available AV evidence contained no detections involving MAN-WS-211 or priya.nair.
- Priya has historical account-compromise activity from August, but NO technical evidence links that historical incident to this October beaconing activity.
- Verdict: False Positive
- Severity: Informational
- No endpoint isolation, account disablement, or destination blocking required based on current evidence.

TUNING:
Do not suppress the beaconing rule globally. If needed, use a narrow exception/reduced priority only for confirmed monitoring-agent traffic where the approved host, destination, and expected heartbeat behavior match. Re-alert on destination, volume, interval, or endpoint-behavior changes.


NEXT SHIFT WATCH ITEMS

Highest priority is renewed CLD-0001 infrastructure activity against Daniel Reeve and the unresolved/escalated Eleanor Walsh Bexley activity.

Watch specifically for:
- 198.51.100.212 or related CLD-0001 infrastructure targeting additional users.
- Any successful authentication involving Daniel Reeve following the blocked attempts.
- Any indication that the standing Conditional Access block is being bypassed.
- New activity from 192.0.2.146 or additional anomalous activity involving ewalsh.ext@cloudora.io.
- Results of Eleanor verification and detailed Bexley SharePoint audit review.
- Continued svc-backup failures outside CHG-2102.
- MAN-WS-211 deviating from the documented monitoring-agent heartbeat pattern.

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
