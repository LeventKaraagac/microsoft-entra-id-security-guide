# Block Legacy Authentication
**Last edited:** 12/10/2026

Legacy authentication means older protocols (such as IMAP, POP and SMTP with a basic password) that can't prompt for multifactor authentication. An attacker who guesses or steals a password can sign in through them with no second check, even if MFA is required everywhere else. Blocking them closes that gap.

## Before you start

- Check the sign-in logs for legacy authentication in use (filter by **Client app**). If an old device or a printer relies on it, it will stop working.
- Your break-glass group (SG-BreakGlass) must exist. See [02](../02-user-group-and-role-structure/).

## Steps

1. Go to **Entra ID > Conditional Access > Policies** and select **New policy**.

<br>
<img src="https://i.imgur.com/JirKOAD.png" alt="Conditional Access Policies page with the New policy option selected"/>
<br>

2. Give the policy a clear name, for example `Block legacy authentication`.

3. Under **Assignments > Users**, include **All users** and exclude your break-glass group.

<br>
<img src="https://i.imgur.com/88pKhdw.png" alt="New policy Users section with All users included"/>
<br>

<br>
<img src="https://i.imgur.com/XwNtF2v.png" alt="Users section Exclude tab with the SG-BreakGlass group selected"/>
<br>

4. Under **Target resources > Resources (formerly cloud apps) > Include**, select **All resources**.

<br>
<img src="https://i.imgur.com/Svl2dp5.png" alt="Target resources section with All resources selected"/>
<br>

5. Under **Conditions > Client apps**, set **Configure** to **Yes** and tick the options under **Legacy authentication clients**.

<br>
<img src="https://i.imgur.com/qRgbn8J.png" alt="Conditions Client apps panel with Configure set to Yes and the legacy authentication clients options ticked"/>
<br>

6. Under **Access controls > Grant**, select **Block access**.

<br>
<img src="https://i.imgur.com/WR0Ui7x.png" alt="Grant controls panel with Block access selected"/>
<br>

7. Set **Enable policy** to **Report-only** and select **Create**.

<br>
<img src="https://i.imgur.com/8OlZppR.png" alt="Enable policy toggle before creating the policy"/>
<br>

The policy now appears in the **Policies** list.

<br>
<img src="https://i.imgur.com/ZlRmBzI.png" alt="Conditional Access Policies list showing the Block legacy authentication policy"/>
<br>

## Test it with What If

Open **What If**, choose a regular user and a legacy client app, and run it. The **Policies that will apply** list should include your new policy.

<br>
<img src="https://i.imgur.com/JAkriSQ.png" alt="What If results with the Block legacy authentication policy listed under Policies that will apply"/>
<br>

## Before you switch it on

Review the sign-in logs. If anything legitimate shows up as a legacy sign-in, fix or retire it first.

## Checklist

- [ ] Legacy authentication use checked in the sign-in logs
- [ ] Policy created, break-glass excluded
- [ ] Tested with What If
- [ ] Switched to On
