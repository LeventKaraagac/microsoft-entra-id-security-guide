# Self-Service Password Reset (SSPR)
**Last edited**: 14/10/2026

Self-service password reset lets users reset their own forgotten passwords without calling IT. It reduces helpdesk work and shortens lockouts, but it also gives an attacker another path into an account, so it needs to be set up with care.

**Where:** Entra ID > Password reset

## 1. Turn it on for everyone

1. Open **Password reset > Properties**.
2. Set **Self-service password reset enabled** to **All** or to selected groups/users.

<br>
<img src="https://i.imgur.com/NpLEYMl.png" alt="Password reset Properties page with Self-service password reset enabled set to All"/>
<br>

For a small company, enabling it for all users is simplest. In a larger tenant you can start with a pilot group, then widen it.

## 2. Require two methods to reset

1. Open **Password reset > Authentication methods**.
2. Set **Number of methods required to reset** to **2** and make sure uncheck **Security questions**. Security questions is prone to phishing and answers can be guessed/found.

<br>
<img src="https://i.imgur.com/eKJkdwu.png" alt="Password reset Authentication methods tab with Number of methods required to reset set to 2"/>
<br>

**Why:** One method is a single point of failure. If an attacker has a phone or a mailbox, one method may be enough to take over the account. Two raise that bar.

> **Admins:** These settings apply to regular users. Administrators are always required to use two methods to reset their passwords.

> **Check your methods list.** Make sure users can actually register two methods you allow. If you disabled Email OTP in [authentication methods](authentication-methods.md), confirm the list still offers at least two methods users can use.

## 3. Turn on notifications

1. Open **Password reset > Notifications**.
2. Turn on **Notify users on password resets?**

<br>
<img src="https://i.imgur.com/71SG21y.png" alt="Password reset Notifications tab with Notify users on password resets turned on"/>
<br>

Users get an email when their password is reset. If it wasn't them, they know right away.

**Admin notifications.** The setting **Notify all admins when other admins reset their password?** is useful in a small company, where it gives you visibility into admin account activity. In a larger organization it is noise, because every admin reset emails every other admin. Turn it on or off to fit your size.

## Check your work

Run a real reset end to end with a test user:

1. Go to the password reset page and enter the test user's email.
2. Complete the verification steps.
3. Set a new password and sign in with it.
4. Confirm the notification email arrived.

## Checklist

- [ ] SSPR enabled for all users
- [ ] Two methods required to reset
- [ ] User notifications on
- [ ] Admin notifications decided
- [ ] End-to-end reset tested with a test user
