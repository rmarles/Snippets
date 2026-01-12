# Exchange Online + Hybrid Notes

Common Exchange Online and mail-flow related snippets and references.

---

## ⚠️ Deprecated: Send-MailMessage

**When:** Legacy scripts  
**Why:** Cmdlet is deprecated

Prefer Microsoft Graph `/sendMail` with an app registration.

---

## Check Mailbox Type

**When:** License / object validation  
**Why:** Identify mailbox class

```powershell
Get-EXOMailbox user@domain.com |
  Select DisplayName, RecipientTypeDetails
```

---

## Find Mailboxes with Forwarding

**When:** Security review  
**Why:** Identify mail exfil paths

```powershell
Get-EXOMailbox -ResultSize Unlimited |
  Where {$_.ForwardingSmtpAddress} |
  Select DisplayName, ForwardingSmtpAddress
```

---

## Message Trace (Last 24 Hours)

**When:** Mail delivery troubleshooting  
**Why:** Validate transport flow

```powershell
Get-MessageTrace -StartDate (Get-Date).AddHours(-24) -EndDate (Get-Date)
```

---

## Mail Diagnostics Links

- https://mha.azurewebsites.net
- https://dmarcian.com/spf-survey/
- https://vamsoft.com/support/tools/spf-policy-tester
