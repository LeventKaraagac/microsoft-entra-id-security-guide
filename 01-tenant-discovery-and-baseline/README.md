# 01 - Tenant Discovery and Baseline
**Last Edited:** 12/10/2026

Before you change anything in Microsoft Entra ID, find out what is already there. This takes about an hour and gives you two things: a clear picture of what you have to work with, and a "before" snapshot to measure your work against.

Open the [Microsoft Entra admin center](https://entra.microsoft.com) with a Global Administrator or Global Reader account. Note what you find for each item below. Don't fix anything yet.

## 1. Licenses
**Where:** Entra ID > Licenses > All products

Check which Entra ID license you have (Free, P1 or P2). This decides what you can build later. For example, risk-based Conditional Access and Privileged Identity Management need P2.
<br>
<img src="https://i.imgur.com/miQ5vJB.png" alt="Licenses overview"/>
<br>

**What to look for:** the license type, and how many are assigned vs available.

## 2. Users
**Where:** Entra ID > Users > All users

List every account, whether it's a Member or Guest, and which ones hold administrator roles. Also look for accounts that shouldn't exist, such as former employees or shared mailboxes with sign-in enabled.

<br>
<img src="https://i.imgur.com/ZsnK28d.png" alt="All users"/>
<br>

## 3. Roles and permissions
**Where:** Entra ID > Roles & administrators

Check who has which role. Pay most attention to **Global Administrator**. Many small tenants have the day-to-day admin account as Global Administrator, which is more access than daily work needs. Also note whether any custom roles exist.

<br>
<img src="https://i.imgur.com/w2sC410.png" alt="Roles and administrators page in Microsoft Entra ID"/>
<br>

## 4. Groups

**Where:** Entra ID > Groups > All groups

Note which groups exist, whether each is Security or Microsoft 365, and how membership is managed. Leave any group that says "do not delete" (for example, the Viva Engage group) alone.

<br>
<img src="" alt="All groups list in Microsoft Entra ID showing the default groups with their group type and membership type"/>
<br>

## 5. Devices

**Where:** Entra ID > Devices > All devices

Check what's registered, whether it's managed, and by what. Entra only shows you what is joined or registered. Device management and compliance belong to Intune (see the [Intune guide](https://github.com/LeventKaraagac/microsoft-intune-device-management-guide)).

<br>
<img src="" alt="All devices page in Microsoft Entra ID showing one registered device with its join type and no management or compliance status"/>
<br>

## 6. Applications

**Where:** Entra ID > Enterprise apps > All applications

List the applications that use your tenant to sign in. For each one, ask whether any Conditional Access policy covers it.

<br>
<img src="" alt="Enterprise applications list in Microsoft Entra ID showing the three applications registered in the tenant"/>
<br>

## 7. Existing Conditional Access policies

**Where:** Entra ID > Conditional Access > Policies

Check whether any policies exist, and if so, what they target and whether each is on, off or report-only. Don't build overlapping policies without knowing what is already enforced.

<br>
<img src="" alt="Conditional Access policies page in Microsoft Entra ID showing an empty list with no policies configured"/>
<br>

## 8. Identity Secure Score

**Where:** Entra ID > Protection > Identity Secure Score

Record the score and the list of recommendations. This is your baseline. Come back to this page when you finish the guide and compare.

<br>
<img src="" alt="Identity Secure Score page in Microsoft Entra ID showing the current score percentage and the list of recommendations grouped by priority"/>
<br>

**Example:** our starting score was [67.5%] with [6] recommendations ([1] high, [2] medium, [3] low priority).

## 9. Define who is responsible

Before you configure anything, decide who owns what. Microsoft's identity framework defines these roles:

| Role | Responsibility | In a small company, usually... |
|---|---|---|
| Sponsor | Approves changes with business impact | Owner / CEO |
| Identity Architect | Designs the identity configuration | IT / security lead |
| Security Owner | Owns security decisions and exceptions | IT / security lead |
| IT Support Manager | Handles day-to-day support | IT lead |
| Compliance Manager | Owns compliance requirements | Owner / operations |

One person can hold several roles in a small company. What matters is that someone is named for each and that the Sponsor signs off on changes that affect how people work.

## 10. Decide your target before you build

Using what you found, write down:
- Which users need administrator roles
- Which groups you need (see [02](../02-user-group-and-role-structure/))
- Which Conditional Access policies you will build now, and which are blocked by licensing or by device management (see [04](../04-conditional-access-policies/))

## Checklist

- [ ] License type recorded
- [ ] All users and admin role holders listed
- [ ] Existing groups, devices and apps noted
- [ ] Existing Conditional Access policies reviewed
- [ ] Identity Secure Score recorded
- [ ] Stakeholders named
