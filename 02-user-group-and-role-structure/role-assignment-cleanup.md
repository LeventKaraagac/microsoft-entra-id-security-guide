# Role Assignment Clean-up
**Last edited:** 12/10/2026

Many tenants start with the main admin account set as **Global Administrator**. That role can change anything in the tenant, which is more access than daily admin work needs. This page moves the account to the lowest role it needs, in an order that never leaves you locked out.

## Why do this

- **Smaller blast radius.** If a Global Administrator account is compromised, the attacker controls the whole tenant. A narrower role limits the damage.
- **Least privilege.** Give each person the lowest role that lets them do their job.
- **Keep Global Administrator for emergencies.** In this guide only the break-glass accounts hold it (see [03](../03-break-glass-accounts/)).

## The order matters

1. Give the daily account the role it actually needs.
2. Create the break-glass accounts and give them Global Administrator.
3. Only then remove Global Administrator from the daily account.

If you remove Global Administrator first and something goes wrong, you may have no account able to fix it.

## Step 1: Add the role the person needs

**Where:** Entra ID > Users > [user] > Assigned roles

Select **Add assignments** and choose the role. In this guide the daily admin needs **Security Administrator**.

<br>
<img src="https://i.imgur.com/DIxHvfE.png" alt="Add assignments panel on a user's Assigned roles tab with Security Administrator selected"/>
<br>

The role now appears in the user's assigned roles table.

<br>
<img src="https://i.imgur.com/a5MIBxB.png" alt="Assigned roles table for the user now listing the Security Administrator role."/>
<br>

## Step 2: Create the break-glass accounts

Follow [03 - Break-glass accounts](../03-break-glass-accounts/). Do not continue until both accounts exist, hold Global Administrator and are in the SG-BreakGlass group.

## Step 3: Remove Global Administrator from the daily account if it still exists by default

**Where:** Entra ID > Users > [user] > Assigned roles

Select the Global Administrator assignment and choose **Remove assignments**.

## Check your work

- The daily admin account shows only the roles it needs.
- Only the break-glass accounts hold Global Administrator.
- You can still sign in to the admin portals with the daily account and do your usual tasks.

## Checklist

- [ ] Daily admin account has the role it needs
- [ ] Break-glass accounts created and in SG-BreakGlass
- [ ] Global Administrator removed from the daily account
- [ ] Assigned roles reviewed: no one else holds Global Administrator
