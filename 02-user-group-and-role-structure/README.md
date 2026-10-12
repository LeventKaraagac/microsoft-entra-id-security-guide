# 02 - User, Group and Role Structure
**Last edited:** 12/10/2026

This page sets up who has access to what: the groups, the users and the administrator roles. Get this right early. Many Conditional Access policies you build later target these groups, so a clean structure here saves rework.

**On this page**
- Planning, groups, users, group members and dynamic device groups (below)
- [Security groups vs Microsoft 365 groups](security-groups-vs-m365-groups.md): which group type and membership type to choose, and why
- [Role assignment clean-up](role-assignment-cleanup.md): moving administrators to least privilege

## 1. Plan before you create anything

Write down each person, their job, which group they belong to, and whether they need an admin role. Keep it short. Example for a small company:

| User | Job title | Admin role | Group |
|---|---|---|---|
| Alex Morgan | IT / Security lead | Security Administrator | SG-Admins |
| Sam Taylor | Owner | None | SG-AllUsers |
| Jordan Lee | Sales manager | None | SG-AllUsers |
| 2 x emergency accounts | n/a | Global Administrator | SG-BreakGlass |
| External contractor | n/a | None | SG-Guests |

Two rules to follow:
- **Least privilege.** Give each person the lowest role that lets them do their job. Day-to-day admin accounts rarely need Global Administrator.
- **Roles go to people deliberately.** Know why each admin role was granted.

## 2. Create the security groups

**Where:** Entra ID > Groups > All groups > New group

<br>
<img src="https://i.imgur.com/Kce8Yxp.png" alt="Groups > All groups page in Microsoft Entra ID with the New group button highlighted"/>
<br>

Choose **Security** as the group type and **Assigned** as the membership type. Fill in a name and description. You can also add **owners**, who can manage the group even if they hold no admin role. See [Security groups vs Microsoft 365 groups](security-groups-vs-m365-groups.md) for why.

<br>
<img src="https://i.imgur.com/vDiPn7e.png" alt="New group form with Security group type, group name, description and Assigned membership type filled in for SG-Admins"/>
<br>

Select **Create**. The group now appears in the Groups list.

<br>
<img src="https://i.imgur.com/ojNr9GR.png" alt="All groups list showing the newly created SG-Admins security group"/>
<br>

### The four groups

Repeat the same steps for each:

| Group | Purpose |
|---|---|
| **SG-Admins** | Accounts with administrator roles |
| **SG-BreakGlass** | Emergency-access accounts, excluded from every Conditional Access policy (see [03](../03-break-glass-accounts/)) |
| **SG-AllUsers** | Standard users. A convenient group for license, app and device-policy assignment (see below) |
| **SG-Guests** | External and guest accounts. Used to scope guest-specific policies |

<br>
<img src="https://i.imgur.com/eltvtTS.png" alt="New group form for SG-BreakGlass with the description Security group for emergency-access accounts excluded from all Conditional Access policies"/>
<br>

<br>
<img src="https://i.imgur.com/JSiJYab.png" alt="New group form for SG-AllUsers with the description Security group for standard licensed users"/>
<br>

<br>
<img src="https://i.imgur.com/9rr344y.png" alt="New group form for SG-Guests with the description Security group for external and guest accounts"/>
<br>

### Do you actually need SG-AllUsers?

For a small company, not strictly. Conditional Access can target **All users** directly, and that is what the baseline policies in this guide use. We still created SG-AllUsers for three reasons:

- **An explicit list.** Some assignments, such as licenses, app access and Intune policies, work with a group you can see and review. "All users" is a built-in target with no membership list.
- **A starting point.** A group you already have is easy to split into department groups later.
- **Consistency.** Every access decision in the tenant then points at a named group.

**Keep baseline policies on "All users", not on this group.** A manually-maintained group goes stale. If someone is created and not added, a policy aimed at the group silently skips them. Baseline protections such as MFA and legacy-authentication blocking should cover everyone by default, with only break-glass excluded.

### Groups per department

As a company grows, create groups per department (for example SG-Sales, SG-Marketing, SG-Finance) when departments need something different, such as:

- Different licenses or app access
- Stricter policies for sensitive teams (for example Finance)
- Different device or data rules

Don't create a group unless it has a purpose, because every extra group is more to maintain. A simple test: if you can't name a license, app or policy that will target the group, you don't need it yet.

You can also fill department groups automatically. A dynamic user group with a rule like `(user.department -eq "Sales")` adds and removes people based on the department field in their profile. This needs Entra ID P1 and only works if the department field is filled in consistently.

## 3. Create users

**Where:** Entra ID > Users > All users > New user > Create new user

<br>
<img src="https://i.imgur.com/D5ArCii.png" alt="All users page with the New user menu open and Create new user selected"/>
<br>

**Basics tab.** Fill in the user principal name, mail nickname, display name and password. You can auto-generate the password. It matters little, because you will require the user to change it at first sign-in.

<br>
<img src="https://i.imgur.com/dJzT1wR.png" alt="Create new user wizard Basics tab with user principal name, display name and an auto-generated password"/>
<br>

**Properties tab.** Fill in the job details. Three fields need care:
- **User type:** *Member* is an employee. *Guest* is an outside person such as a contractor who needs access.
- **Employee type:** full-time, part-time, contractor and so on.
- **Manager:** you can pick one now, or set it later if the manager's account doesn't exist yet.

<br>
<img src="https://i.imgur.com/FK25MxE.png" alt="Create new user wizard Properties tab showing user type, job title, employee type and manager fields"/>
<br>

**Assignments tab.** Add the user to a group, an administrative unit or a role. You can add up to 20 group or role assignments here.

Group assignment:

<br>
<img src="https://i.imgur.com/SuaBuKd.png" alt="Assignments tab with the Add group button and the SG-AllUsers group selected"/>
<br>

Role assignment:

<br>
<img src="https://i.imgur.com/QfTSP6W.png" alt="Assignments tab with the Add role button and a directory role being selected"/>
<br>

**Review + create.** Check the details, then select **Create**.

<br>
<img src="https://i.imgur.com/SWmnang.png" alt="Review + create tab summarising the new user's settings before creation"/>
<br>

The new user now appears under **Users > All users**.

<br>
<img src="https://i.imgur.com/5Sog6Q9.png" alt="All users list showing the newly created user"/>
<br>

## 4. Fix administrator roles

Existing tenants often have the main admin account set as Global Administrator. Move it to least privilege, but in the right order. See [Role assignment clean-up](role-assignment-cleanup.md).

## 5. Add members to groups

If you didn't assign groups during user creation, do it here.

**Where:** Entra ID > Groups > All groups > [group] > Members > Add members

<br>
<img src="https://i.imgur.com/EIb44Xk.png" alt="Group page Members tab with the Add members button highlighted"/>
<br>

The picker can add users, groups, devices and enterprise applications, because a group can contain any of these. For these groups, use the **Users** tab and select the right people.

<br>
<img src="https://i.imgur.com/2EKuLiT.png" alt="Add members panel with the Users tab open and several users selected"/>
<br>

After adding, the members appear on the **Members** tab.


## 6. Create dynamic device groups

Dynamic groups need the Entra ID **P1** license or higher. They place devices into groups automatically, using rules on device attributes. This guide groups devices by operating system and ownership:

| Group | Contains |
|---|---|
| SG-Devices-Windows | Windows devices |
| SG-Devices-Linux | Linux devices |
| SG-Devices-iOS-Corporate | Company-owned iPhones |
| SG-Devices-iOS-BYOD | Personally owned iPhones |

**Disclaimer:** The table above is specific for an organization. It can look different for your organization according to your needs for deployment.

**Where:** Entra ID > Groups > All groups > New group

<br>
<img src="https://i.imgur.com/m0Ze5hW.png" alt="New group button in the All groups page, used to start creating a dynamic device group"/>
<br>

Choose **Security** as the type, name the group, add a description and set **Membership type** to **Dynamic Device**.

<br>
<img src="https://i.imgur.com/M79L6kz.png" alt="New group form with Security type and Dynamic Device membership type selected for SG-Devices-Linux"/>
<br>

Under **Dynamic device members**, select **Add dynamic query**. On the **Configure rules** tab, pick the property, operator and value. For an OS-based group, use `deviceOSType`, `Equals`, and the OS name.

<br>
<img src="https://i.imgur.com/HPwhbHY.png" alt="Dynamic membership rule builder with property deviceOSType, operator Equals and value Linux"/>
<br>

Use the **Validate rules** tab before you save. Select **Add devices**, choose some of your devices and confirm the rule includes the ones you expect.

<br>
<img src="https://i.imgur.com/b8JA9N6.png" alt="Validate rules tab with the Add devices option used to select test devices"/>
<br>

<br>
<img src="https://i.imgur.com/H8g6aNG.png" alt="Validate rules results showing the Linux test device as a member of the group"/>
<br>

Save the rule and create the group. Open it and check that the matching device appears automatically.

<br>
<img src="https://i.imgur.com/CPywNrC.png" alt="SG-Devices-Linux group page showing the Linux device added automatically by the dynamic rule"/>
<br>

### Rules for all four groups

**SG-Devices-Windows**
```
(device.deviceOSType -eq "Windows")
```

**SG-Devices-Linux**
```
(device.deviceOSType -eq "Linux")
```

**SG-Devices-iOS-Corporate**
```
(device.deviceOSType -eq "iPhone") and (device.deviceOwnership -eq "Company")
```

**SG-Devices-iOS-BYOD**
```
(device.deviceOSType -eq "iPhone") and (device.deviceOwnership -eq "Personal")
```

> **Check your values.** The exact `deviceOSType` text can differ by platform and enrollment method. Always run **Validate rules** against a real device before you rely on a rule.

## Checklist

- [ ] People, roles and groups planned
- [ ] SG-Admins, SG-BreakGlass, SG-AllUsers and SG-Guests created
- [ ] Users created and placed in the right group
- [ ] Daily admin account moved to the lowest role it needs ([role assignment clean-up](role-assignment-cleanup.md))
- [ ] Dynamic device groups created and rules validated
