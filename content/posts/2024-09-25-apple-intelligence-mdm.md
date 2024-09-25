+++
title = "Managing Apple Intelligence via MDM"
date = "2024-09-25"
author = "Guillaume Ross"
description = "Apple Intelligence is a collection of features, and so far, they can be controlled very granularly."
+++

This summer, since WWDC, I was repeatedly asked:

> How do I disable Apple Intelligence on our company laptops?

Apple Intelligence didn't exist yet. Not even in the betas, plus, Apple is pretty strict around betas and NDAs. 
The features were clearly going to roll out in stages throughout the year, so my answer was always, “Yeah, you’ll probably be able to control each one via MDM once the new settings drop.”

Now, with the release of iOS 18 and macOS 15, we've got some of those settings to play with. 
Apple Intelligence shows up in iOS 18.1 and macOS 15.1, so you can prepare.

As with most settings that make the device less useful, they require a supervised device, so forget doing that on BYOD.

The settings are in [restrictions](https://developer.apple.com/documentation/devicemanagement/restrictions). By searching for "iOS 18 and later" on the page, we can find the new settings, most of which are Apple Intelligence related:

| Setting                             | Description                                                                                                                                |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| allowGenmoji                        | If false, prohibits creating new Genmoji. Requires a supervised device. Available in iOS 18 and later.                                     |
| allowImagePlayground                | If false, prohibits the use of image generation. Requires a supervised device. Available in iOS 18 and later and macOS 15 and later.       |
| allowImageWand                      | If false, prohibits the use of Image Wand. Requires a supervised device. Available in iOS 18 and later.                                    |
| allowPersonalizedHandwritingResults | If false, prevents the system from generating text in the user’s handwriting. Requires a supervised device. Available in iOS 18 and later. |
| allowWritingTools                   | If false, disables Apple Intelligence writing tools. Requires a supervised device. Available in iOS 18 and later and macOS 15 and later.   |

Most people asking me how to disable Apple Intelligence this summer were specifically worried about ChatGPT/OpenAI integration, which has not been released yet. 

Based on those restrictions, it's safe to assume a similar setting will be added to iOS before the feature is released.