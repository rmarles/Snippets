# macOS (Apple) – Support & Recovery Notes

Snippets and procedures related to Apple macOS devices, with a focus on
enterprise-managed (Jamf-enrolled) Macs.

---

## Reset Local Password Using FileVault Recovery Key (Jamf-managed Mac)

**When:**  
- You cannot log into a Jamf-managed Mac  
- The local macOS password is unknown or out of sync  
- You have access to the **FileVault personal recovery key**

**Why:**  
This allows you to regain access to the local macOS account **without**
changing the user’s cloud identity password (Entra ID / AD / IdP).

> ⚠️ This procedure resets **only the local macOS password**.

---

### High-level flow

1. Restart the Mac  
2. Trigger the password reset workflow at the login screen  
3. Authenticate using the FileVault recovery key  
4. Set a new local password  

---

### Step-by-step

#### 1. Restart the Mac

- Reboot the device normally.

---

#### 2. Trigger password recovery

At the macOS login screen:

1. Click the affected **user account**
2. When prompted for the password, select **“Forgot password?”**

> Note:  
> - This option may only appear **after 2–3 failed login attempts**  
> - On some macOS versions, it appears as:  
>   - “Reset it using your recovery key”  
>   - or after a short delay beneath the password field

---

#### 3. Choose recovery key reset

- Select **“Reset using recovery key”**
- macOS will prompt for the **FileVault personal recovery key**

Enter the recovery key:
- Exactly as shown
- Include hyphens
- Case-insensitive, but spacing matters

---

#### 4. Set a new local password

macOS will prompt you to:

- Enter a **new local password**
- Confirm the password
- Optionally set a password hint

This action:
- ✅ Resets the **local macOS account password**
- ❌ Does **not** reset the cloud / directory password
- ❌ Does **not** affect Jamf enrollment

---

## Important notes

- If FileVault is **disabled**, this method will not work
- If the recovery key is **unknown or invalid**, escalation is required
- The user may still be prompted to:
  - Re-sync keychain items
  - Re-authenticate cloud services (OneDrive, Outlook, etc.)

---

## Post-recovery checks (recommended)

After successful login:

- Verify FileVault status:
  - System Settings → Privacy & Security → FileVault
- Confirm Jamf check-in:
  - Open **Self Service**
  - Ensure policies run successfully
- Advise the user to:
  - Update their cloud password if required
  - Sign out/in of affected apps

---

## Troubleshooting

### “Forgot password?” option does not appear

**Checks:**
- Confirm you selected the **user icon**, not “Other”
- Trigger additional failed login attempts
- Restart again and retry

---

### Recovery key rejected

**Checks:**
- Verify the key in Jamf matches the device
- Confirm hyphens and character order
- Ensure this is the **personal** recovery key, not an institutional key

---

### User can log in but apps keep prompting for passwords

**Why:**  
Local password changed, but keychain still uses the old password.

**Fix:**  
- Open **Keychain Access**
- Reset login keychain (last resort)

---

## Scope reminder

This procedure:
- ✅ Restores local macOS access
- ❌ Does not fix directory / cloud auth issues
- ❌ Does not rotate FileVault keys automatically

Jamf policies may rotate keys on next check-in.
