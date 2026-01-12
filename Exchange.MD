# Exchange Online + Hybrid Notes

Common Exchange Online and mail-flow related snippets and references.

---

## ⚠️ Deprecated: Send-MailMessage

**When:** Legacy scripts  
**Why:** Cmdlet is obsolete and doesn’t support modern auth well

Prefer Microsoft Graph `sendMail` via Graph PowerShell (`Send-MgUserMail`).

---

## ✅ Modern: Send email with Microsoft Graph PowerShell (Send-MgUserMail)

**When:** Sending reports/alerts without SMTP basic auth  
**Why:** Uses Graph permissions and modern auth; works in PowerShell 7+

**Prereqs:**
- Microsoft Graph PowerShell (`Microsoft.Graph`)
- Graph permission: `Mail.Send` (delegated) or `Mail.Send` application permission (app-only)
- A mailbox-enabled sender

```powershell
# Install-Module Microsoft.Graph -Scope CurrentUser

# Delegated (interactive)
Connect-MgGraph -Scopes "Mail.Send"

$FromUPN = "sender@domain.com"
$ToUPN   = "recipient@domain.com"

$Subject = "Automation report"
$BodyHtml = "<h3>Report</h3><p>Hello from Graph.</p>"

$params = @{
  Message = @{
    Subject = $Subject
    Body = @{
      ContentType = "HTML"
      Content     = $BodyHtml
    }
    ToRecipients = @(
      @{ EmailAddress = @{ Address = $ToUPN } }
    )
  }
  SaveToSentItems = $true
}

Send-MgUserMail -UserId $FromUPN -BodyParameter $params
```

**Notes:**
- If you're sending as another mailbox, the sender needs Send As / Send on Behalf, and your Graph permissions must align.
- For app-only, authenticate with a certificate/secret and ensure admin consent for `Mail.Send`.

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

---

# Troubleshooting

## Graph mail send fails (403 / Insufficient privileges)

**When:** Send-MgUserMail returns 403  
**Why:** Missing scopes/admin consent or wrong auth mode

**Checks:**
```powershell
Get-MgContext
```
- Confirm `Mail.Send` is present (delegated) or granted (application)
- Confirm the sender mailbox exists and is licensed
