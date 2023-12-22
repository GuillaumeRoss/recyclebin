+++
title = "AutoSpill the beans: not THAT big a deal"
date = "2023-12-22"
author = "Guillaume Ross"
description = "The AutoSpill vulnerability has been discussed in social media and on online publications. Scaring people from using password managers is net-negative."
+++

Research can be interesting, useful for risk management purposes, helpful to those building software and even entertaining. 

Sometimes, the usefulness of research is undone by the public discourse on a security issue.

In the last few weeks, you’ve heard of “[AutoSpill](https://i.blackhat.com/EU-23/Presentations/EU-23-Gangwal-AutoSpill-Zero-Effort-Credential-Stealing.pdf)”, a vulnerability affecting multiple password managers on Android.

I’ve heard everything about this vulnerability on LinkedIn. The posts are often accompanied by a AI-Generated image of a crying Android baby and broken padlocks.

Quotes ranged from calls to warn everyone about this important vulnerability to “you should never use password manager browser extensions” (paraphrased).

When we make these types of claims, as security professionals, we risk scaring people away from a tool that reduces the risk they are exposed to.

If a single person stopped using a password manager and reverted to a text file, to using the same password everywhere, to using an “algorithm” in their heads to generate “different” passwords, that person is now less secure due to the discourse of security professionals on social media. 

## The facts about the vulnerability

1. Autofill vulnerabilities aren't new. Yes, they're a risk, but the majority of autofill action happens on legitimate sites, preventing phishing. Drama much?
2. Most password managers ask before spilling the beans. Calm down, it’s not a complete free-for-all.
3. AutoSpill needs a malicious app to work. It's not a web-based 0day in the wild. Walled gardens like App Store or Play Store keep most nasties out. 
4. Password managers are patching or have patched this. Fearmongers, please take a seat. By the time you switch to `Password123$` or to your “elite brain generated algorithm”, it'll be a non-issue.

