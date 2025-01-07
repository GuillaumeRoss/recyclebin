+++
title = "Magic Links: Annoying Links"
date = "2025-01-06"
author = "Guillaume Ross"
description = "Subscription websites now like to use magic email links for login. They are extremely annoying."
+++

"Magic Links", a term previously used to refer to a [futuristic PDA](https://en.wikipedia.org/wiki/Magic_Link) is now used to refer to the least magic thing possible, an email with a link. Last week, the great website you should subscribe to if you haven't already, [404 Media](https://www.404media.co/), posted ["We Don't Want Your Password"](https://www.404media.co/we-dont-want-your-password-3/) in defense of so-called magic links. It's another form of "passwordless". 

This article makes some good points around such links. They are much harder to phish than passwords. The platform 404 uses ([Ghost](https://ghost.org/)) doesn't need to do a good job at hashing passwords, since the passwords aren't stored, which protects people who re-use passwords across websites in the event the platform got compromised and used weak hashing. It protects the website from people reusing passwords on other sites getting breached.

The article even covers some of my annoyances with this system, which I'll list below, but it throws out this sentence:

> We find this to be a much easier login process

This is only true for very straightforward scenarios, where the end user does not have a password manager (which are built-in to every OS and browser at this point).

### Cases when a passkey, or username+password is better than "magic" links:

1. Logging in to a device that doesn't have my email account already logged in, where I do NOT want to expose all my email to the device. This is commonly referred to as "wanting to read an article from my work laptop".
1. Logging in to browsers that are within applications, like my RSS reader, on mobile, is a pain or simply impossible. This also occurs if you're using in-app browsers of social media apps, when clicking on links.
1. Logging in from browsers that aren't the default browser. Now I need to copy a link from my email, paste it in a URL bar.
1. Saving the website as a "web app" on the iOS homescreen is impossible.       
1. SMTP delays are still a thing. Rarer, maybe, but it still happens. Same with SMS codes.
1. When all the conditions are right, logging in still takes multiple times longer than it would with a passkey, or even a username and password.
1. Connectivity isn't always great. I've been in situations

So, please, make your case that you do not want to store passwords because it's easier not to and has some security benefits over old school password practices, but don't tell me it's "easier". 

In fact, another annoying system is to email an OTP the end user can type in. While it is also annoying, it at least allows you to easily log in in situations where you don't have a clear and easy path from the email client to the browser you want to log in to. [Stratechery](https://stratechery.com/), powered by Passport, uses this type of scheme (click link OR type in OTP), which is still shifting annoyances onto end-users to free developers from implementing passkeys, but at least has a bit more of an appreciation for end-users.


{{< goatcounter >}}