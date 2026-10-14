# Secure Security Info Registration
**Last edited:** 14/10/2026

When users register MFA methods or set up self-service password reset, they are adding ways to sign in to the account. This policy protects that moment. It treats registration itself as a protected action, so a stranger with only a password can't register their own MFA method on someone else's account.

## The risk it closes

Imagine a new account that hasn't registered MFA yet. If the password is stolen first, the attacker could register their own authenticator before the real user does, and lock the real user out. Requiring strong authentication before registering stops that.

New users can still register safely using a [Temporary Access Pass](https://learn.microsoft.com/en-us/entra/identity/authentication/howto-authentication-temporary-access-pass#create-a-temporary-access-pass): a time-limited credential you give them to complete their first registration.

## Before you start

- Turn on and review the Temporary Access Pass method in [authentication methods](../06-authentication-methods-and-sspr/authentication-methods.md), so new users have a way to register.
- Keep your break-glass group excluded. See [03](../03-break-glass-accounts/).

## Steps

1. Go to **Entra ID > Conditional Access > Policies** and select **New policy**. Name it, for example, `Secure security info registration`.

2. Under **Assignments > Users > Include**, select **All users**. Under **Exclude**, add your guest and external users and your break-glass group.

<br>
<img src="https://i.imgur.com/QACqnjv.png" alt="Policy Users section with All users included and guest and external users and the SG-BreakGlass group excluded"/>
<br>

3. Under **Target resources**, change the target type to **User actions** and select **Register security information**.

<br>
<img src="https://i.imgur.com/OB910Ov.png" alt="Target resources section set to User actions with Register security information ticked"/>
<br>

4. Under **Access controls > Grant**, select **Grant access**, tick **Require authentication strength** and choose the appropriate built-in or custom strength from the list.

<br>
<img src="https://i.imgur.com/XKIW0VD.png" alt="Grant controls panel with Require authentication strength ticked and an authentication strength selected"/>
<br>

5. Review your settings, set **Enable policy** to **Report-only**, and select **Create**.

<br>
<img src="https://i.imgur.com/q020nDy.png" alt="Conditional Access Policies list showing the Secure security info registration policy"/>
<br>

## Before you switch it on

1. Make sure new users can get a Temporary Access Pass from an admin, or they won't be able to register.
2. Test with a new test account: create it, issue a Temporary Access Pass, and complete registration.
3. Switch the policy to **On**.

## Checklist

- [ ] Temporary Access Pass available
- [ ] Policy created, guests and break-glass excluded
- [ ] Tested with a new test account
- [ ] Switched to On
