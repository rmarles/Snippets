# Exchange Online + Sync'd RElated Items

---
This is deprecated - need to modernize:

```
        $mailSettings = @{
            Subject    = $Subject
            From       = $From
            SmtpServer = $smtpRelay
            Encoding   = [text.encoding]::UTF8
            To         = $EmailAddress
            Cc         = $CopyAddress
            Bcc        = $BlindCopyAddress
            Body       = $Body
            Priority   = "High"
        }

        $mailSettings.Body = $mailSettings.Body.replace("%displayName%",$ADObject.displayName)

        try {
            Send-MailMessage @mailSettings -BodyAsHtml -ErrorAction Stop
        } catch {
            Write-Host "[ERROR] "+$_.Exception.Message
        }
```

---

In a previous life as a DNS admin I was responsible for mail delivery. Going through my old notes I found a few handy links.
Mail Header Analyzer
https://mha.azurewebsites.net
SPF Tools
https://dmarcian.com/spf-survey/
https://vamsoft.com/support/tools/spf-policy-tester
