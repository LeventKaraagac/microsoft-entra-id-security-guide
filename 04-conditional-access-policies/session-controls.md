# Sign-in Frequency and Browser Persistence for Admins portals and for everyone
**Last edited:** 14/10/2026

Session controls decide how often users must sign in again, and whether a sign-in survives closing the browser. Shorter sessions are safer for sensitive apps. Longer sessions are more convenient on trusted devices. This page builds two policies:

1. [Sign-in frequency for admin portals](#policy-1-sign-in-frequency-for-admin-portals): admins must re-authenticate on a schedule when they use the Microsoft admin portals
2. [Sign-in frequency and browser persistence](#policy-2-sign-in-frequency-and-browser-persistence-for-everyone): a baseline for every user across every resource

## Before you start

- Create your break-glass group first (see [03](../03-break-glass-accounts/)). Both policies exclude it.
- Think about how this will feel for users. These policies make people sign in again, so test them with a small group before you enforce.

## Policy 1: Sign-in frequency for admin portals

Admin portals are where a stolen session does the most damage. This policy forces re-authentication on a schedule when someone uses them, even if they already have a valid session.

It targets only the admin portals, because sign-in frequency can be set per app. You can't do the same with browser persistence (see Policy 2).

1. Go to **Entra ID > Conditional Access > Policies** and select **New policy**. Name it `Sign-in frequency for Admin Portals`.

2. Under **Assignments > Users > Include**, select **All users**. Only administrators can reach the admin portals, but targeting the resource instead of a group means any account that gains admin access later is covered too. Under **Exclude**, add your break-glass group.

<br>
<img src="https://i.imgur.com/7MhEvOD.png" alt="Policy Users section with All users included for the admin portals sign-in frequency policy"/>
<br>

3. Under **Target resources > Resources (formerly cloud apps)**, choose **Select resources** and pick **Microsoft Admin Portals**, which covers all of them.

<br>
<img src="https://i.imgur.com/fOeINx8.png" alt="Target resources section with Select resources chosen and Microsoft Admin Portals selected"/>
<br>

4. Under **Access controls > Session**, tick **Sign-in frequency**, choose **Periodic reauthentication** and set it to **[X hours]**.

<br>
<img src="https://i.imgur.com/4u4mPfD.png" alt="Session controls panel with Sign-in frequency ticked, Periodic reauthentication selected and the frequency set"/>
<br>

5. Set **Enable policy** to **Report-only** and select **Create**. The policy now appears in the **Policies** list.

<br>
<img src="https://i.imgur.com/aTZY9zx.png" alt="Conditional Access Policies list showing the Sign-in frequency for Admin Portals policy"/>
<br>

## Policy 2: Sign-in frequency and browser persistence for everyone

This policy applies to every user and every resource. It sets two independent controls:

- **Persistent browser session: Never persistent.** The session ends as soon as the browser is closed, regardless of how much time has passed.
- **Sign-in frequency.** While the browser stays open, users must sign in again after a set time, regardless of activity.

**Why this policy targets All resources.** Browser persistence is controlled by one session token shared by every tab and app in a browser session. It can't be narrowed to a subset of apps, so the policy has to cover **All resources**. For the same reason, the baseline sign-in frequency setting lives in this policy rather than in a separate one.

1. Create a new policy named `Sign-in frequency and browser persistence`.

2. Under **Assignments > Users > Include**, select **All users**. Under **Exclude**, add your break-glass group.

<br>
<img src="https://i.imgur.com/QrglgsJ.png" alt="Policy Users section with All users included for the browser persistence policy"/>
<br>

3. Under **Target resources > Resources (formerly cloud apps) > Include**, select **All resources**.

<br>
<img src="https://i.imgur.com/VgItzXI.png" alt="Target resources section with All resources selected"/>
<br>

4. Under **Access controls > Session**:
   1. Tick **Sign-in frequency**, choose **Periodic reauthentication** and set it to **[X hours or days]** (Default is 90 days, 7 days is recommended for a stricter policy).
   2. Tick **Persistent browser session** and choose **Never persistent**.

<br>
<img src="https://i.imgur.com/FDWTmGd.png" alt="Session controls panel with Sign-in frequency set to periodic reauthentication and Persistent browser session set to Never persistent"/>
<br>

5. Set **Enable policy** to **Report-only** and select **Create**.

<br>
<img src="https://i.imgur.com/tbH9LV2.png" alt="Conditional Access Policies list showing the Sign-in frequency and browser persistence policy"/>
<br>

## How the two policies work together

The admin portal policy applies only to the admin portals. The baseline policy applies to everything. When more than one policy sets a sign-in frequency for the same sign-in, the most restrictive value applies.

A common pattern is a longer baseline for ordinary apps and a shorter period for admin portals, so the most sensitive access is re-checked most often. Pick values that match how your users actually work.

## Test them

Use the **What If** tool: choose an admin, select an admin portal as the target app, and confirm both policies appear under **Policies that will apply**. Then choose a regular user and a normal app, and confirm only the baseline policy applies.

## Before you switch them on

1. Check the sign-in logs for how often users would be prompted.
2. Consider the impact on people who work in the same browser all day. If the baseline is too disruptive, lengthen it.
3. Switch each policy to **On** one at a time, starting with the admin portal policy.

## Checklist

- [ ] Admin portals policy created, break-glass excluded
- [ ] Baseline policy created, break-glass excluded
- [ ] Both tested with What If
- [ ] Report-only results reviewed
- [ ] Both switched to On
