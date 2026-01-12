# Exchange Online + Hybrid Notes

Common Exchange Online and mail-flow related snippets and references.

---

## ⚠️ Deprecated: Send-MailMessage

**When:** Legacy scripts  
**Why:** `Send-MailMessage` is obsolete and doesn’t support modern auth well.  
**Use instead:** SMTP via **MailKit** (relay or authenticated), or Microsoft Graph.

---

## ✅ Modern SMTP (MailKit – relay friendly)

**When:** Scripts that previously used `Send-MailMessage` with an SMTP relay  
**Why:** MailKit is the supported, modern replacement for `System.Net.Mail.SmtpClient`

### Prereq (once)
```powershell
Install-Module MailKit -Scope CurrentUser
```

### Example body template

```powershell
$Body = @"
<html>
<body>
<p>Hello %displayName%,</p>
<p>This message was generated automatically.</p>
<p><strong>Do not reply.</strong></p>
</body>
</html>
"@
```

### Send mail (MailKit)

```powershell
$mailSettings = @{
    Subject    = $Subject
    From       = $From
    SmtpServer = $smtpRelay
    Port       = 25
    EnableSsl  = $false
    To         = $EmailAddress
    Cc         = $CopyAddress
    Bcc        = $BlindCopyAddress
    Body       = $Body
    Priority   = "High"
}

# token replacement
$mailSettings.Body = $mailSettings.Body.Replace("%displayName%", $ADObject.DisplayName)

function Add-Addresses {
    param($List, $Addresses)
    if (-not $Addresses) { return }
    foreach ($a in ((@($Addresses) -join ';') -split '[;,]' | ForEach-Object { $_.Trim() } | Where-Object { $_ })) {
        $List.Add([MimeKit.MailboxAddress]::Parse($a)) | Out-Null
    }
}

$msg = [MimeKit.MimeMessage]::new()
Add-Addresses $msg.From $mailSettings.From
Add-Addresses $msg.To   $mailSettings.To
Add-Addresses $msg.Cc   $mailSettings.Cc
Add-Addresses $msg.Bcc  $mailSettings.Bcc

$msg.Subject = $mailSettings.Subject
$msg.Body = [MimeKit.TextPart]::new("html") { Text = $mailSettings.Body }

if ($mailSettings.Priority -eq "High") {
    $msg.Headers["Importance"] = "high"
    $msg.Headers["X-Priority"] = "1"
}

$smtp = [MailKit.Net.Smtp.SmtpClient]::new()
try {
    $smtp.Connect($mailSettings.SmtpServer, $mailSettings.Port, $mailSettings.EnableSsl)
    $smtp.Send($msg)
    $smtp.Disconnect($true)
}
catch {
    Write-Host "[ERROR] $($_.Exception.Message)"
}
finally {
    $smtp.Dispose()
}
```

---

## ✅ Modern API-based mail (Microsoft Graph)

**When:** No SMTP relay available or modern auth is required  
**Why:** Uses Microsoft Graph permissions and OAuth

```powershell
Connect-MgGraph -Scopes "Mail.Send"

$FromUPN = "sender@domain.com"
$ToUPN   = "recipient@domain.com"

$Subject  = "Automation report"
$BodyHtml = "<p>Hello from Graph.</p>"

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

---

## Check Mailbox Type

```powershell
Get-EXOMailbox user@domain.com |
  Select DisplayName, RecipientTypeDetails
```

---

## Find Mailboxes with Forwarding

```powershell
Get-EXOMailbox -ResultSize Unlimited |
  Where-Object { $_.ForwardingSmtpAddress } |
  Select DisplayName, ForwardingSmtpAddress
```

---

## Message Trace (Last 24 Hours)

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

```powershell
Get-MgContext
```
