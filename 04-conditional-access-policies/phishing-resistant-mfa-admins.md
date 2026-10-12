# Require Phishing-Resistant MFA for Administrators
**Last edited:** 12/10/2026

Administrator accounts are the most valuable targets in a tenant. This policy requires them to sign in with a **phishing-resistant** method, such as a passkey (FIDO2) or Windows Hello for Business, which can't be tricked out of the user on a fake sign-in page the way a code or push approval can.

## Before you start

- Admins must have a phishing-resistant method registered before you enforce this. If they don't, they will be locked out. Keep the policy in **report-only** until each admin has registered a passkey.
- You need your break-glass group excluded so you can recover if something goes wrong. See [03](../03-break-glass-accounts/).
- Check the licensing and registration for the methods you allow in [authentication methods](../06-authentication-methods-and-sspr/authentication-methods.md).

## Steps

1. Go to **Entra ID > Conditional Access > Policies** and select **New policy**. Name it, for example, `Require phishing-resistant MFA for administrators`.

2. Under **Assignments > Users > Include**, pick who the policy covers:
   - **Directory roles** to cover specific admin roles (best for most tenants, since it follows role assignment), or
   - **Users and groups** to pick people or a group such as SG-Admins.

<br>
<img src="https://i.imgur.com/ipfLFBU.png" alt="Policy Users section with Select users and groups chosen and directory roles being selected for inclusion"/>
<br>

3. Under **Exclude**, add your break-glass group.

4. Under **Target resources > Resources (formerly cloud apps) > Include**, select **All resources**.

<br>
<img src="https://i.imgur.com/btjmPat.png" alt="Target resources section with All resources selected"/>
<br>

5. Under **Access controls > Grant**, select **Grant access**, tick **Require authentication strength** and choose **Phishing-resistant MFA**.

<br>
<img src="https://i.imgur.com/r2FJ8LS.png" alt="Grant controls panel with Require authentication strength ticked and the Phishing-resistant MFA strength selected"/>
<br>

6. Set **Enable policy** to **Report-only** and select **Create**.

<br>
<img src="https://i.imgur.com/4ybqxRE.png" alt="Conditional Access Policies list showing the Require phishing-resistant MFA for administrators policy"/>
<br>

## Test it with What If

Open **What If**, choose an admin user and an admin app such as Intune, and run it. The new policy should appear under **Policies that will apply**.

<br>
<img src="https://i.imgur.com/mrRk0v6.png" alt="What If results for an administrator signing in to Intune, with the phishing-resistant MFA policy listed as applying"/>
<br>

## Before you switch it on

1. Make sure every admin covered by the policy has registered a passkey or other phishing-resistant method.
2. Review the sign-in logs for the report-only result on real admin sign-ins.
3. Test with one admin account first, then switch the policy to **On**.

> **Heads up:** Some device clients can't satisfy a phishing-resistant requirement. If an admin signs in from one, they may need a temporary exclusion until the client or method is supported. Document any exception and review it regularly. See the [Intune guide](https://github.com/LeventKaraagac/microsoft-intune-device-management-guide) for a Linux example.

## Checklist

- [ ] Phishing-resistant method registered for every admin
- [ ] Policy created, break-glass excluded
- [ ] Tested with What If
- [ ] Report-only results reviewed
- [ ] Switched to On
