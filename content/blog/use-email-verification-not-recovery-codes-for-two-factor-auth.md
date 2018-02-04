+++
blogimport = true
categories = ["blog"]
date = "2016-10-13T14:46:00Z"
draft = true
title = "Use Email Verification, Not Recovery Codes for Two-Factor Auth"
updated = "2016-10-13T14:46:15.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
A friend of mine recently lost access to an account protected by Two-Factor Authentication because he was forced to do a factory reset on his phone, and he couldn't find his recovery codes anywhere. By design, this means it is now completely impossible for him to access his own account. However, I realized that he's now in a situation where he has access to the attached e-mail account, *and* he knows the current account password.

*This doesn't make any sense*. We already treat e-mail as an inconsistent 3rd factor of authentication for the purpose of password resets. When you have two-factor authentication enabled and you need to reset your password because you forgot it, you have to enter your two-factor auth code **and** you must then click on a link from an e-mail sent to you to prove that you have control over the e-mail account. The purpose here is to verify two separate methods of authentication so that you can reset the third. That's why it's called **two-factor** authentication, not *smartphone authentication*.

Of course, many people point out that e-mail is very insecure. This is true, but... we use it to reset passwords *anyway*. Whether anyone wants to admit it, e-mail is being used as a third authentication factor, despite how insecure it is. Two-factor authentication with backup codes simply moves your single point of failure to your backup codes, which you have to store *somewhere*. A security professional will probably lock them up in a heavily encrypted airgapped vault, but what is a normal person going to do? Put them in their wallet? Isn't that exactly what we tell them to do?

With Security Fatigue setting in, it's harder than ever to convince people to enable two-factor authentication, and yet it is simultaneously much more likely for their passwords to be compromised. In order to be protected, people need to enable two-factor auth for their accounts, but we shouldn't be asking them to then store a bunch of single-factor backup codes in a super secret place. Since we're *already* using e-mail + two-factor to reset the password, we might as well use e-mail + password to reset the two-factor auth, or at the very least make this an option. Many companies try to send codes over SMS to avoid this issue, but everyone knows that SMS can be trivially redirected, which simply destroys your two-factor authentication entirely. Instead of sending codes over SMS, we should be allowing two-factor auth resets using both the e-mail and the current password, because that *is* two factor authentication. It's just using a temporary e-mail link instead of a temporary code. 
