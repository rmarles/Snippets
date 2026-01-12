# Entra ID Notes

Practical observations and limitations encountered with Microsoft Entra ID.

---

## SAML Claim Padding Limitation

**When:** Building SAML claims  
**Why:** Padding is unsupported

SAML claim transforms do not support string padding.

Feedback:
https://feedback.azure.com/d365community/idea/dbfd5ded-ea25-ec11-b6e6-000d3a4f06a4

---

## Cloud Group Name Limitation in Claims

**When:** Group-based claims  
**Why:** Cloud groups emit IDs only

Only synced AD groups can flow display names.

Feedback:
https://feedback.azure.com/d365community/idea/32805ab0-b025-ec11-b6e6-000d3a4f0789

---

## Handy Portal Index

**When:** Navigating Microsoft portals

https://msportals.io/

---

## Token Inspection

**When:** Auth troubleshooting  
**Why:** Decode tokens

https://jwt.ms

---

# Troubleshooting

## Dynamic Group Not Updating

**When:** Membership doesn't change as expected  
**Why:** Processing delays, rule issues, or incorrect assumptions

**Checks:**
- Validate rule syntax in portal
- Confirm rule processing status and last updated time
- Test with a known user that should match / not match
