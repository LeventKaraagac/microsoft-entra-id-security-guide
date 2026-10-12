# 04 - Conditional Access Policies
**Last edited:** 12/10/2026

Conditional Access is Microsoft's [Zero Trust policy engine](https://learn.microsoft.com/en-us/security/zero-trust/deploy/identity). It takes signals such as who is signing in, from where, on what device and to which app, and decides whether to allow, block or ask for more proof. This page builds a baseline set of policies for a small company.

Set up your groups first ([02](../02-user-group-and-role-structure/)) and your break-glass accounts ([03](../03-break-glass-accounts/)). Every policy below excludes the **SG-BreakGlass** group, so those accounts can always get in if a policy goes wrong.

## Documentation status

| Policy | Documentation |
|---|---|
| Block legacy authentication | [Published](block-legacy-authentication.md) |
| Require phishing-resistant MFA for administrators | [Published](phishing-resistant-mfa-for-admins.md) |
| Require MFA for all users | [Published](mfa-for-all-users.md) |
| Secure security info registration | [Published](secure-security-info-registration.md) |
| Sign-in frequency and browser persistence | [Published](session-controls.md) |
| Require MFA for guests | In-progress |
| Require MFA for risky sign-ins | In-progress |
| Require remediation for risky users | In-progress |
| Require authentication strength for device registration | In-progress |
| Require device compliance | In-progress |
| Require compliant device, hybrid joined, or MFA | In-progress |
| Require MFA for Azure management | Planned |
| Block access for unknown or unsupported devices | In-progress |
| Require terms of use to be accepted | In-progress |
| Block access by location | In-progress |

## Start in report-only mode

Every policy here starts in **Report-only** mode. The policy is evaluated on each sign-in and the result is logged, but nothing is blocked. This lets you see who would have been affected before you enforce anything.

When you are comfortable with the results, switch the policy to **On**. You can also roll it out in stages by including a pilot group first.

**How to review the impact:** open **Entra ID > Monitoring & health > Sign-in logs**, select a sign-in, and look at the **Conditional Access** tab. It shows each policy and whether it would have succeeded or failed. See [Microsoft's report-only documentation](https://learn.microsoft.com/en-us/entra/identity/conditional-access/howto-conditional-access-insights-reporting).

## Creating a policy

**Where:** Entra ID > Conditional Access > Overview, then **Create new policy** or **Create new policy from templates**

<br>
<img src="https://i.imgur.com/JirKOAD.png" alt="Conditional Access Overview page with the Create new policy and Create new policy from templates options"/>
<br>

Microsoft also provides ready-made templates. Open **Create new policy from templates** to see them. The pages below build each policy by hand so you understand every setting.

<br>
<img src="https://i.imgur.com/VU25gvP.png" alt="Conditional Access policy templates page listing the available Microsoft templates grouped by scenario"/>
<br>

## Testing a policy with What If

The **What If** tool tests how your policies would apply to a given sign-in, without anyone actually signing in.

**Where:** Entra ID > Conditional Access > Policies > What If

Pick a user, an app and the conditions to test, then select **What If**. The results list the policies that would apply. Use it on every policy.

## Checklist

- [ ] Groups and break-glass accounts ready
- [ ] Each policy created in report-only mode
- [ ] Each policy tested with What If
- [ ] Sign-in logs reviewed before switching any policy to On
