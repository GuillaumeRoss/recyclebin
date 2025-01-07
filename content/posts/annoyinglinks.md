+++
title = "Magic/Tragic Email Links: Don't make them the only option"
date = "2025-01-06"
author = "Guillaume Ross"
description = "Subscription websites now like to use magic email links for login. They are extremely annoying."
+++

The term "Magic Links" once meant a [futuristic PDA](https://en.wikipedia.org/wiki/Magic_Link). Nowdays, companies like [Auth0](https://auth0.com/docs/authenticate/passwordless/authentication-methods/email-magic-link) use it to refer to the slightly-magical feat of including a login link in an email.

Last week, the great website you should subscribe to if you haven't already (it's great, when you're not logged out), [404 Media](https://www.404media.co/), posted ["We Don't Want Your Password"](https://www.404media.co/we-dont-want-your-password-3/) in defense of so-called magic links. 

Of course, as stated in the article, such email links are harder to phish than passwords, can't lead to a breach of passwords, and protect the site itself against users who might reuse passwords previously compromised.

The article even covers some of my annoyances with this system, but throws out this sentence:

> [We find this to be a much easier login process and wish it was more common across the web where appropriate.](https://www.404media.co/we-dont-want-your-password-3/)

Easier than what? Easier than a long password, without a password manager? Easier than a passkey? Easier than an OTP sent to the same email address? 

This sentence reads to me as one written by someone mostly working and _living_ from a single laptop and mobile device. The second part of the sentence, calling for more sites to do this is why I am writing this.

For any scenario with a minimal amount of complexity, like users with multiple computers, and you're looking at a scenario where the site's unwillingness to deal with other login methods shoves friction on the end-user. 


### What makes them tragic:

1. Multiple devices. Who doesn't use at least a few computers weekly? I don't have my email on my gaming PC, nor do I have it on my work laptops.
1. Slower. From 2 seconds slower to minutes slower, depending on SMTP delays as well as how awkward it is to get the link to the right browser.
1. Anti-mobile. As mentioned by 404 in their own article, this breaks the ability to use in-app browsers, which is quite annoying especially for RSS reader type apps. It makes interacting with any local link in the RSS feed extremely annoying.
1. Indirect security downsides. Pushing people to access personal email on work devices (or vice-versa) isn't exactly a win for security.

Another annoying _passwordless_ system is to email or SMS an OTP the end user can type in. 

While this sucks, it at least allows you to easily log in in situations where you don't have a clear and easy copy/paste path from the email client to the browser you want to log in to. 

[Stratechery](https://stratechery.com/), powered by [Passport](https://passport.online), uses this type of scheme (click link OR type in OTP), which is still shifting annoyances onto end-users to free developers from implementing passkeys, but at least has a bit more of an appreciation for end-users.

If you insist on using magic/tragic links by default, at least consider offering a robust alternative, such as [passkeys](https://fidoalliance.org/passkeys/), especially if your audience is technical and privacy-focused.


{{< goatcounter >}}