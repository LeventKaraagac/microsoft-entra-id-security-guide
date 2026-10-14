# 06 - Authentication Methods and Self-Service Password Reset
**Last Edited:** 14/10/2026

Conditional Access decides *when* a user must prove who they are. This section decides *how* they are allowed to prove it. If weak methods stay enabled, a strong policy can still be satisfied with a weak sign-in method, so do this hardening before or alongside the policies in [04](../04-conditional-access-policies/).

**In this section**
- [Authentication methods](authentication-methods.md): review which sign-in and MFA methods are enabled, and turn off the weak ones
- [Self-service password reset](self-service-password-reset.md): let users reset their own passwords safely

## The idea in one paragraph

Not all multifactor methods are equal. A push approval or passkey is hard to phish. A six-digit code, whether from an app or an email, can be tricked out of a user on a fake sign-in page, and an email code is only as secure as the mailbox it goes to. The goal is to leave users the strongest methods that fit your company and remove the weakest.

## Before you start

- You need the **Authentication Policy Administrator** role or higher (or Global Administrator).
- Decide which methods your users can actually use. Disabling a method people rely on will lock them out of registering or resetting.
- Keep your break-glass accounts working. See [03](../03-break-glass-accounts/).

## Checklist

- [ ] Current authentication methods reviewed
- [ ] Weak methods disabled or restricted
- [ ] Self-service password reset enabled and configured
- [ ] Policies tested with a real test user
