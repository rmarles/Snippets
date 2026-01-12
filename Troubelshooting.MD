# Troubleshooting Notes

Common issues and reminders across identity, Graph, and M365.

---

## Graph: Insufficient Privileges

**Symptom:** 403 / Insufficient privileges  
**Check:** Scopes + admin consent

```powershell
Get-MgContext
```

---

## Empty Results

**Symptom:** No output  
**Check:** Requested properties

Use `-Property` explicitly.

---

## Dynamic Group Not Updating

**Check:**
- Rule syntax
- Processing status
- Test user inclusion

---

## Mail Flow Issues

**Check:**
- Message trace
- SPF / DKIM / DMARC
- Forwarding rules
