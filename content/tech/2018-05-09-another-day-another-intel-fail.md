---
id: 76
title: Another Day, Another Intel Fail
date: 2018-05-09T14:18:35+00:00
author: Ciarán Ainsworth
type: post
guid: https://rootkey.co.uk/?p=76
permalink: /2018/05/09/another-day-another-intel-fail/
categories:
  - Technology
tags:
  - adobe
  - flash
  - intel
  - meltdown
  - microsoft
  - patch
  - spectre
  - tuesday
---
A few years ago, I&#8217;d have been shocked to see the sort of vulnerabilities I see announced these days once a year, let alone once a month. But the rise of cybersecurity as a respected industry has led to the big companies such as Microsoft, Apple, Canonical, etc. pulling up their socks and crushing vulnerabilities in a timely manner. This is all great, except for those of us whose job it is to deploy these fixes.

<!--more-->

The big news out of this week&#8217;s Patch Tuesday was undoubtedly the announcement of [yet another flaw with Intel&#8217;s x86 architecture](https://www.theregister.co.uk/2018/05/09/intel_amd_kernel_privilege_escalation_flaws/). Following on from the disastrous [Meltdown and Spectre](https://meltdownattack.com/), the latest vulnerability allows applications to hijack code and read memory due to a mistake in the implementation of kernel-level code. Nota bene, I am not a programmer or a chip expert, this is just what I&#8217;ve gleaned from El Reg&#8217;s far better explanation of the issue. What is more interesting to me is that this is not a flaw with x86 _per se_, but rather &#8220;the error appears to be due to developer interpretation of existing documentation.&#8221;

### Bad Documentation is Bad

If an individual reads a set of instructions and manages stumble along the way, that&#8217;s understandable and usually rectifiable. However, if everybody reading the instructions fails in the same way, the instructions are bad. End of story. This cannot be passed off as being developer error if the documentation was misleading or unclear. It can be presumed that Microsoft, Google, Apple, and companies/communities working on *BSD and Linux are pretty smart cookies indeed. I feel it is unlikely that all of them could have failed in the same way without being misled.

So what does this mean? I would personally be taking a good long look at Intel after the disastrous year they&#8217;ve been having and wondering whether or not we had made a huge mistake entrusting our standards and architectures to a company so driven by quick wins and marketing that they will happily sacrifice security and the long-term health of the industry. As their recent &#8220;meltdown&#8221; has shown us, they have been a company obssessed with being able to claim the fastest and bestest at the cost of sane design. And let&#8217;s face it: [these chips aren&#8217;t really getting that much better.](https://www.cnet.com/news/intel-kaby-lake-7th-gen-7700-7600-7350/)

It seems that [Apple&#8217;s recent decision to move away from Intel architecture](https://www.bloomberg.com/news/articles/2018-04-02/apple-is-said-to-plan-move-from-intel-to-own-mac-chips-from-2020) may not be such a crazy idea. Certainly, Intel is faster _now_, but it is only a matter of time and investment before others surpass them. Certainly, they&#8217;ve not made themselves many friends this year. This month&#8217;s vuln is naught more than another eye-roll for sysadmins and users at this point, a fault for which developers are being unfairly blamed.

### Anything Else?

Flash Player, of course, has [yet another high vulnerability announcement](https://helpx.adobe.com/security/products/flash-player/apsb18-16.html). Time to get your Adobe hat on and grumble all the way to deployment on this one. Or better yet, get rid of it entirely and see how many people actually complain. Most of the rest of the serious stuff (17/21 critical) seems to be browser vulnerabilities. Par for the course&#8230;
