# Authentication Methods
**Last edited:** 14/10/2026

This page reviews which sign-in and multifactor methods your tenant allows, and turns off the weak ones.

**Where:** Entra ID > Authentication methods > Policies

## 1. Review what is enabled

Open the Policies page and note the status of each method. A new tenant usually has these enabled by default:

- Passkey (FIDO2)
- Microsoft Authenticator
- Temporary Access Pass
- Software OATH tokens
- Email one-time passcode

<br>
<img src="https://i.imgur.com/XPTWUwQ.png" alt="Authentication methods Policies page in Microsoft Entra ID listing Passkey (FIDO2), Microsoft Authenticator, Temporary Access Pass, Software OATH tokens and Email OTP"/>
<br>

## 2. How to change a method

Select a method to open its policy. The **Enable and target** tab controls who can use it: you can include or exclude users and groups. The **Configure** tab holds the settings specific to that method.

<br>
<img src="https://i.imgur.com/NEkzPk6.png" alt="Authentication method policy page showing the Enable and target tab with include and exclude options and the Configure tab"/>
<br>

## 3. Disable Microsoft Authenticator OTP

Microsoft Authenticator can approve a sign-in with a push or number-matching prompt. It can also show a plain six-digit code, called the Authenticator OTP.

Open **Microsoft Authenticator > Configure** and turn **Allow use of Microsoft Authenticator OTP** off.

**Why:** A time-based code is a weaker fallback than a push or passwordless approval. It can be phished like any other code. Leave the other settings on their defaults.

<br>
<img src="https://i.imgur.com/JEGcdyH.png" alt="Microsoft Authenticator policy Configure tab with Allow use of Microsoft Authenticator OTP turned off"/>
<br>

## 4. Disable Email one-time passcode

Open **Email OTP** and set it to **Disabled**.

**Why:** If someone's mailbox is compromised, an emailed code lets the attacker pass MFA. Email is the weakest link.

<br>
<img src="https://i.imgur.com/btP3X6Q.png" alt="Email OTP authentication method policy with the Enable switch turned off"/>
<br>

> **Check first.** Email codes can also be used for password reset and for sign-in by external (guest) users. Confirm you don't rely on this for either before disabling.

## 5. Restrict Software OATH tokens

Software OATH tokens are codes from an authenticator app such as Google Authenticator or Microsoft Authenticator. They are phishable in the same way as Email OTP, though stronger because they don't depend on email.

Rather than turning them off entirely, allow them only for a group that has a real need:

1. Open **Software OATH tokens**.
2. On the **Enable and target** tab, change the target from **All users** to **Select groups**.
3. Add the group that needs them.

<br>
<img src="https://i.imgur.com/ExUNq1v.png" alt="Software OATH tokens policy Enable and target tab with the target changed from All users to a selected group"/>
<br>

## What to leave on

- **Microsoft Authenticator** with push and number matching, as your main MFA method
- **Passkey (FIDO2)**, the strongest option and required for phishing-resistant MFA (see [phishing-resistant MFA for administrators](../04-conditional-access-policies/phishing-resistant-mfa-admins.md))
- **Temporary Access Pass**, so new users can register their first methods safely (see [secure security info registration](../04-conditional-access-policies/secure-security-info-registration.md))

## Check your work

- Sign in as a test user and register a method. Confirm the disabled methods are not offered.
- Confirm an admin can still register a passkey or Authenticator.
- Confirm your break-glass accounts can still sign in.

## Checklist

- [ ] Methods reviewed
- [ ] Authenticator OTP disabled
- [ ] Email OTP disabled
- [ ] Software OATH tokens restricted to a group
- [ ] Tested with a non-admin test user
