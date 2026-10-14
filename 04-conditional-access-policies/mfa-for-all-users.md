# Require MFA strength for All Users
**Last edited:** 14/10/2026

A password alone isn't enough. This policy requires every user to pass multifactor authentication, which stops most account takeovers that start with a stolen or guessed password.

## Before you start

- Check [authentication methods](../06-authentication-methods-and-sspr/authentication-methods.md) so users have strong methods available to register.
- Plan the rollout. New users should register MFA before they hit the requirement. [Secure security info registration](secure-security-info-registration.md) and a Temporary Access Pass help with this.
- Keep your break-glass group excluded. See [03](../03-break-glass-accounts/).

## Steps

1. Go to **Entra ID > Conditional Access > Policies** and select **New policy**. Name it, for example, `Require MFA for all users`.

2. Under **Assignments > Users > Include**, select **All users**. Under **Exclude**, add your break-glass group.

<br>
<img src="https://i.imgur.com/8gKPhQt.png" alt="Policy Users section with All users included and the SG-BreakGlass group excluded"/>
<br>

3. Under **Target resources > Resources (formerly cloud apps) > Include**, select **All resources**.

<br>
<img src="https://i.imgur.com/HQ9sdmT.png" alt="Target resources section with All resources selected"/>
<br>

4. Under **Access controls > Grant**, select **Grant access**, tick **Require authentication strength** and choose **Multifactor authentication**.

<br>
<img src="https://i.imgur.com/Dhjz3tF.png" alt="Grant controls panel with Require authentication strength ticked and the Multifactor authentication strength selected"/>
<br>

5. Set **Enable policy** to **Report-only** and select **Create**.

<br>
<img src="https://i.imgur.com/mEr3xM5.png" alt="Conditional Access Policies list showing the Require MFA for all users policy"/>
<br>

## Why authentication strength and not "Require multifactor authentication"

Authentication strength lets you control which MFA methods count. A built-in strength such as *Multifactor authentication* accepts a defined set of methods, and you can later tighten it with a stricter or custom strength.

## Before you switch it on

1. Check the sign-in logs. Anyone who has not registered MFA will be asked to when they next sign in. Warn users first.
2. Test with a pilot group, then switch the policy to **On**.

## Checklist

- [ ] Users warned and able to register MFA
- [ ] Policy created, break-glass excluded
- [ ] Report-only results reviewed
- [ ] Switched to On
