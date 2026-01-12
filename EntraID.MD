# Entra ID Notes

Practical observations and limitations encountered with Microsoft Entra ID.

---

## SAML Claim Padding Limitation

**When:** Building SAML claims  
**Why:** Padding is unsupported

Recently I needed to include a 10-digit padded employee number in a SAML claim.
It seems that padding is a missing function within SAML Claim Transforms.
I tried a few things, like joining 000000000 to the employee ID (say 12345) then returning the rightmost 10 characters (0000012345), RegEx expressions, etc, but nothing seemed to work as expected. I even opened a Microsoft ticket on this, and there isn’t a solution for padding in the transforms.
Please visit Add claim manipulation for padding · Community (azure.com) to upvote this.

Feedback:
https://feedback.azure.com/d365community/idea/dbfd5ded-ea25-ec11-b6e6-000d3a4f06a4

---

## Cloud Group Name Limitation in Claims

**When:** Group-based claims  
**Why:** Cloud groups emit IDs only

Currently, Microsoft does not support flowing a group name of a cloud group in their SAML claims. Interestingly, you can flow the name if the group is AD synchronized.
I get object IDs are immutable, however, sometimes it’s just better to flow a friendly name.
If this has impacted you as well, upvote the following: Group name instead of group id for SAML

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
