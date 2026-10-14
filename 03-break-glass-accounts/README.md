# 03 - Break-Glass Accounts
**Last edited:** 14/10/2026

A break-glass account (also called an emergency-access account) is a highly privileged account kept for one situation: you are locked out and no normal admin can get back in. Without one, a Conditional Access mistake, an MFA outage or the loss of your only admin can leave you locked out of your own tenant.

Set these up **before** you build the Conditional Access policies in [04](../04-conditional-access-policies/), and before you remove Global Administrator from any daily account ([role assignment clean-up](../02-user-group-and-role-structure/role-assignment-cleanup.md)) if this is your first time creating users and diving into configuring Entra ID as the main admin account will have Global Administrator as a role by default.

## The rules

| Rule | Why |
|---|---|
| Create **two** accounts | If one is lost, locked or its credentials are damaged, the other still works |
| Make them **cloud-only** and use the tenant's `.onmicrosoft.com` domain | They don't depend on your custom domain, DNS, federation or on-premises directory |
| Don't tie them to a person | They shouldn't disappear when someone leaves, and the credentials shouldn't belong to one person |
| Assign **Global Administrator** permanently | They need to be able to fix anything. Don't make the role eligible through PIM, because that needs a working sign-in first |
| Use them **only** in an emergency | Normal admin work happens on named admin accounts |
| Protect the credentials offline | Don't store them in the same systems that a lockout would block |
| Alert on every sign-in | Any use of these accounts should be a deliberate, noticed event |

## 1. Create the accounts

**Where:** Entra ID > Users > All users > New user > Create new user

1. On the **Basics** tab, use the `.onmicrosoft.com` domain for the user principal name. Choose a name that isn't tied to a person, and consider one that doesn't advertise its purpose to someone browsing your user list.
2. Generate a long, random password. You'll store it offline in a later step.

3. On the **Assignments** tab, add the account to the **SG-BreakGlass** group and assign the **Global Administrator** role. Make the role assignment permanent (active), not eligible.

4. Select **Review + create**, then **Create**.

Repeat for the second account.

## 2. Add both accounts to SG-BreakGlass

If you didn't add them during creation, go to **Entra ID > Groups > SG-BreakGlass > Members > Add members** and add both. See [02](../02-user-group-and-role-structure/) for the group steps.

<br>
<img src="https://i.imgur.com/zVv4YMX.png" alt="SG-BreakGlass group Members tab listing the two emergency-access accounts"/>
<br>

## 3. Exclude the group from your Conditional Access policies

In every Conditional Access policy you build, add **SG-BreakGlass** under **Assignments > Users > Exclude**. That way a misconfigured policy can't lock out the very accounts you'd use to fix it. Each page in [04](../04-conditional-access-policies/) repeats this step.

After you build your policies, check with the **What If** tool that a break-glass account is not targeted by any of them.

> **Because these accounts skip your policies, protect them in other ways.** Use strong credentials, keep them offline, monitor them and review them (sections 4 to 6).

## 4. Sign-in methods: use a passkey

Microsoft now requires multifactor authentication for sign-in to the Azure portal, the Microsoft Entra admin center and the Intune admin center, and this applies to every account, including emergency ones. A password-only break-glass account may be unable to sign in to those portals.

Microsoft recommends **phishing-resistant** methods for emergency accounts: register a **passkey (FIDO2 security key)** on each account, or use certificate-based authentication. Keep each key in a separate, secure physical location. Check Microsoft's current guidance on [emergency access accounts](https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/security-emergency-access) before you finalise, since the requirements are changing.

Make sure the passkey method is enabled for these accounts in your [authentication methods](../06-authentication-methods-and-sspr/authentication-methods.md).

## 5. Store the credentials

- Print or write the password and keep it somewhere physical and secure, such as a safe. If you store it electronically, use an encrypted store that does not depend on this tenant.
- Keep the two accounts' credentials in **different** places, so one incident can't take out both.
- Document who is allowed to retrieve them and under what circumstances.

## 6. Monitor every sign-in

Any sign-in to a break-glass account should trigger an alert to at least two people. At minimum:

1. Find the sign-ins in **Entra ID > Monitoring & health > Sign-in logs**, filtered by user.
2. Send the sign-in logs to a Log Analytics workspace, and create an alert rule that fires when either account signs in. If you use a SIEM such as Microsoft Sentinel, build a detection rule for the same thing.

## 7. Test and review

- **Review them** at least twice a year: who knows where the credentials are, are the passkeys still where they should be, and has anything changed that could affect them.
- **After every use**, change the password and re-secure the credentials.

## Checklist

- [ ] Two cloud-only accounts created on the `.onmicrosoft.com` domain
- [ ] Both hold a permanent Global Administrator assignment
- [ ] Both are in SG-BreakGlass
- [ ] SG-BreakGlass excluded from every Conditional Access policy
- [ ] Passkey (FIDO2) or another phishing-resistant method registered
- [ ] Credentials stored offline in separate locations
- [ ] Sign-in alert configured and tested
- [ ] Accounts tested, with a review date set
