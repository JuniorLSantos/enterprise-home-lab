# Windows Security Event Auditing

## Overview

This document describes the investigation of Windows Security events generated
during the controlled account-lockout test performed in
`corp.juniorlab.test`.

The objective was to correlate failed authentication attempts, the resulting
account lockout and the later administrative unlock. This reproduces a common
help desk and security-analysis workflow using Windows Event Viewer.

---

## Environment

| Component | Configuration |
|---|---|
| Domain | corp.juniorlab.test |
| Domain controller | DC01 |
| DC01 address | 192.168.10.10 |
| Operating system | Windows Server 2022 |
| Log analyzed | Windows Logs > Security |
| Test account | `CORP\policy.test` |
| Administrative account | `CORP\junior.admin` |

The test account was a disposable, non-administrative identity created for the
previous domain account-security validation. It was disabled after testing.

---

## Investigation Method

The Security log on DC01 was filtered to the last 24 hours using these event
IDs:

```text
4625,4740,4767,4771,4776
```

| Event ID | Meaning |
|---:|---|
| 4625 | An account failed to log on |
| 4740 | A user account was locked out |
| 4767 | A user account was unlocked |
| 4771 | Kerberos pre-authentication failed |
| 4776 | Domain controller attempted to validate account credentials |

Filtering only changes which records are displayed. It does not delete or
modify the Security log.


The captured sequence contained the three events required to reconstruct the
test: repeated 4625 failures, followed by 4740 and then 4767.

---

## Failed Authentication — Event 4625

Event 4625 recorded an unsuccessful authentication attempt for
`policy.test`.

| Field | Observed value | Interpretation |
|---|---|---|
| Account Name | `policy.test` | Identity used in the attempt |
| Logon Type | `3` | Network logon |
| Failure Reason | Unknown user name or bad password | General failure description |
| Status | `0xC000006D` | Logon failed because the credentials were invalid |
| Sub Status | `0xC000006A` | The account name was valid, but the password was incorrect |
| Keywords | Audit Failure | The authentication operation failed |


The status gives the general authentication result, while the substatus
provides the more specific cause. Reading both fields avoids treating every
4625 event as the same type of failure.

### Network and Authentication Details

| Field | Observed value |
|---|---|
| Workstation Name | `DC01` |
| Source Network Address | `192.168.10.10` |
| Source Port | `61208` |
| Logon Process | `NtLmSsp` |
| Authentication Package | `NTLM` |


The source was DC01 because the controlled invalid-credential attempts were
generated locally on the server. Logon type 3 shows that Windows processed the
request as network authentication rather than an interactive sign-in at the
desktop.

---

## Account Lockout — Event 4740

After the configured threshold of ten invalid attempts was reached, event
4740 recorded the account lockout.

| Field | Observed value |
|---|---|
| Account That Was Locked Out | `CORP\policy.test` |
| Caller Computer Name | `DC01` |
| Subject Account | `CORP\DC01$` |
| Logged | October 5, 2026 at 08:26:49 |
| Keywords | Audit Success |


The trailing `$` in `DC01$` identifies it as the Active Directory computer
account for the server. `Audit Success` means Windows successfully audited the
account-management operation; it does not mean that the attempted logon
succeeded.

---

## Administrative Unlock — Event 4767

Event 4767 recorded the manual unlock performed after the lockout test.

| Field | Observed value |
|---|---|
| Administrator | `CORP\junior.admin` |
| Target Account | `CORP\policy.test` |
| Logged | October 5, 2026 at 08:28:38 |
| Computer | `DC01.corp.juniorlab.test` |
| Keywords | Audit Success |


This event provides administrative accountability by identifying both the
target account and the privileged identity that performed the unlock.

---

## Reconstructed Timeline

| Time | Event | Finding |
|---|---:|---|
| 08:26:49 | 4625 | Repeated invalid NTLM network authentications for `policy.test` |
| 08:26:49 | 4740 | `policy.test` was locked after reaching the configured threshold |
| 08:28:38 | 4767 | `junior.admin` unlocked `policy.test` |

The three event types correlate into one complete incident: invalid
credentials were submitted, the domain enforced its lockout policy and an
authorized administrator restored access.

---

## Operational Notes

- This activity was a read-only review of existing logs; no policy or system
  configuration was changed.
- No snapshot was required because Event Viewer filtering does not modify the
  virtual machine.
- Event timestamps, account names, source host and authentication details
  should be correlated before drawing a conclusion.
- A single 4625 event may be a normal typing error. Repeated failures followed
  by 4740 are stronger evidence of a lockout incident.
- The test used a disposable standard account rather than an administrative or
  personal identity.

---

## Lessons Learned

- Event Viewer can reconstruct authentication and account-management activity
  without installing an additional monitoring platform.
- Event 4625 identifies failed logons and includes useful status, source and
  authentication fields.
- Event 4740 confirms that Active Directory enforced the configured account
  lockout threshold.
- Event 4767 identifies the administrator responsible for restoring access.
- `Audit Success` and `Audit Failure` describe whether the audited operation
  succeeded; they are not general security verdicts.
- Correlating multiple events provides more reliable evidence than analyzing
  an isolated log entry.

---

## Next Steps

- Forward Windows Security events to Splunk.
- Create searches for failed logons, account lockouts and administrative
  unlocks.
- Build an authentication-monitoring dashboard.
- Test event generation from CLIENT01 to distinguish workstation and server
  source information.
