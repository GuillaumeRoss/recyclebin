+++
title = "Before Preaching, Stop Punching Yourself"
date = "2024-10-04"
author = "Guillaume Ross"
description = "Before You Make Any More Cybersecurity Awareness Content, Fix These Outdated Practices"
+++

*Originally posted on the [JupiterOne blog](https://www.jupiterone.com/blog/marketing-wouldnt-let-me-call-this-before-preaching-stop-punching-yourself).*

Some seem to love Cyber Security Awareness Month. Of course, at least as many despise it, because it makes no sense to have intense focus for one month and then forget about it for the other 11. 

Without saying if I love it or if I hate it, I decided to write a set of rules. If any of this is a problem in your organization, you HAVE to fix it before you are allowed to fill up user’s inboxes with more “awareness”.

Have you recently told someone working in HR to not open attachments in email? Have you told someone to not click things on the [thing-clicking machine](https://x.com/swagitda_/status/1503751776134180873)?

Here are signs you shouldn’t hire some “creatives” to generate “content” to “educate users” this year:

# Your company blocks copy/paste on a website login form

Are you making any person using a password manager to log in to your service hate computers, your company AND behave less securely by blocking paste for “security reasons” ? 

Are you driving people to install [browser extensions](https://github.com/jswanner/DontF-WithPaste) just to deal with the completely irrational behavior your login form exhibits?

Why? Do you WANT people to reuse passwords from other sites, making sure their accounts on your service will get compromised, or do you simply want people to use simple passwords that are easier to crack? 

Did an auditor tell you to do this, and nobody dared argue?

Go fix this before you tell any of your employees or customers ANYTHING about cyber security awareness month.

# Password expiration

Because you want your helpdesk to suffer, you make passwords expire. Sure, there is absolutely no security benefit to it because people only rotate the last digit in the password, but in 2006, a [QSA](https://en.wikipedia.org/wiki/Qualified_Security_Assessor) told you to do this every 90 days.

NIST started recommending that practice to end years ago. This fall, they’ve made that guidance stronger.

If your employees or customers have to rotate passwords at a certain frequency, you are hereby prohibited from celebrating October Cyber Security Awareness Month.

# No password manager for employees
Are you really going to send an animated video of poor quality to people, instructing them not to reuse passwords, while simultaneously not providing them with an easy to use password manager? 

Slow down, Satan!

# Weak or no MFA

Stop blaming people for leaking credentials when [technical solutions](https://fidoalliance.org/fido2/) exist. Imagine if cars came with a button that made the airbags pop, and car manufacturers repeatedly told you not to press it. Just REMOVE THE FACE PUNCHING BUTTON!


# Your HR departments sends emails indistinguishable from phishing
Explaining to someone what is and is not phishing is very difficult. Most people do not know that email can be sent without authentication. Many of you reading this are already thinking of yelling at me in your heads “But but SPF! DKIM/DMARC! Hasn’t this guy heard of BIMI?”. Alright.

But in reality, spam will also come from domains that aren’t the right one. Sure, it’s not exactly the right domain, but it looks like it. Or it’s a completely unrelated domain, with a reply-to that seems to be legit.

How do you explain any of this to someone and then expect them not to be confused when the HR department sends out a survey asking for sensitive information, hosted on a free SurveyMonkey page that isn’t on a subdomain of your company? Or when your support team does the same to a customer, using some support SaaS tool that isn’t on your domain?

# Laptops that can’t survive hotel Wi-Fi

VPN VPN VPN VPN VPN VPN VPN. Why do so many people sound like a sponsored Youtube video?

If you’re recommending that people use a VPN when using “public Wi-Fi” because you’re afraid of data being sniffed or modified in transit, spend some time [hardening browsers](https://chromeenterprise.google/policies/#SSLErrorOverrideAllowed) and laptops, and ensure you do not provide any service over unencrypted protocols like POP3 or IMAP without TLS.

Once you’re done with that, when you get the urge to yell “VPN” again at a random person using a laptop at Starbucks, read [this](https://www.cs.umd.edu/~akgul/papers/vpn-ads.pdf) to relax instead.

# Authenticating callers at the help desk with security questions

Security questions are just passwords, but worse. Not only are they not safe, they’re not user friendly. How would I remember what I said my favorite dessert is? And if I said “poutine”, then everyone knows anyway.

Worse, relying on “private” information like SSNs. Social security numbers for pretty much everyone have been leaked at least once, so why are we using this to authenticate anyone?

Instead of having everyone in your company waste an hour looking at videos about not putting passwords on post-it notes, rework your password reset process to use multifactor authentication.

# Next? Time to celebrate!

If you did get rid of one of these outdated practices, you’re now allowed to celebrate by sharing with your employees and/or customers why you did it, and how they’re now safer AND more productive.

You’ve used October Cyber Security Awareness month as an excuse to improve cyber security. 

Well done!