# Security Groups vs Microsoft 365 Groups
**Last edited:** 12/10/2026

When you create a group in Microsoft Entra ID, the first choice is the **group type**. The second is the **membership type**. This page explains both, and what this guide chooses.

## Group type

| | Security group | Microsoft 365 group |
|---|---|---|
| Main use | Access control | Collaboration |
| Targets Conditional Access, role assignments, app and license permissions | Yes | Yes |
| Creates a shared mailbox, Teams site and SharePoint library | No | Yes |
| Best for | Policies and permissions | Teams that share files, email and chat |

**Security group.** Used purely for access control: targeting Conditional Access policies, role assignments, and app and license permissions. No collaboration features are attached.

**Microsoft 365 group.** Built for collaboration. Creating one automatically provisions a shared mailbox, a Teams site and a SharePoint library for the members.

### What this guide uses

**Security groups.** The groups here exist to scope Conditional Access policies and role assignments. A Microsoft 365 group would also create a mailbox, a Teams site and a SharePoint library nobody asked for, which adds clutter and extra things to secure.

If you also need a shared mailbox or a team space, create a Microsoft 365 group separately for that. Don't use it for access control.

For more detail, see [Microsoft's group documentation](https://learn.microsoft.com/en-us/entra/fundamentals/concept-learn-about-groups).

## Membership type

| Type | How members are added | Best for |
|---|---|---|
| **Assigned** | You add and remove them yourself | Small groups that rarely change |
| **Dynamic user** | A rule on user attributes adds and removes them automatically | Department or job-based groups at scale |
| **Dynamic device** | A rule on device attributes adds and removes them automatically | Grouping devices by OS or ownership |

Dynamic membership needs Entra ID P1 or higher.

### What this guide uses

- **Assigned** for the four user groups (SG-Admins, SG-BreakGlass, SG-AllUsers, SG-Guests). They are small and change rarely, and you want to control exactly who is in the admin and break-glass groups.
- **Dynamic device** for the device groups, because devices appear and change without anyone updating a list. See [section 6 of the main page](README.md).

### A caution about dynamic groups

A dynamic rule is only as good as the data it reads. A department rule like `(user.department -eq "Sales")` places nobody in the group if the department field is empty or spelled inconsistently. Always check a new rule on real accounts before you rely on it.

## Owners

Whichever type you pick, you can add **owners** to a group. An owner can manage that group's membership even without an admin role. Be careful with this on sensitive groups: whoever owns SG-Admins can add themselves or others to it. Keep owners on those groups to a minimum.

## Quick decision guide

- Need to control access, policies or roles? **Security group.**
- Need a shared mailbox, chat and files for a team? **Microsoft 365 group.**
- Small group that rarely changes? **Assigned.**
- Large or fast-changing group tied to an attribute? **Dynamic** (with P1).
