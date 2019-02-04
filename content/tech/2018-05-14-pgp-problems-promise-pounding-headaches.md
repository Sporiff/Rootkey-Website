---
id: 98
title: PGP Problems Promise Pounding Headaches
date: 2018-05-14T12:01:15+00:00
author: Ciarán Ainsworth
type: post
guid: https://rootkey.co.uk/?p=98
permalink: /2018/05/14/pgp-problems-promise-pounding-headaches/
categories:
  - Technology
tags:
  - efail
  - email
  - enigmail
  - gnupg
  - gpg
  - information
  - infosec
  - pgp
  - security
  - vulnerability
---
<img style="float: left; display: inline;" src="https://rootkey.co.uk/wp-content/uploads/2018/05/Term2.png" alt="" width="250" height="250" align="left" />Waking up to news of serious vulnerabilities is never fun, much less so when those issues reside within a critical product used to protect sensitive information in potentially dangerous situations. While the idea of encrypting electronic communications using GPG has been a matter of some debate for a while – with many users claiming its relative difficulty and clunky implementation makes it difficult for casual users to use – its importance in the world of journalism and situations in which privacy is essential is undisputed. It’s therefore distressing when the EFF themselves <a href="https://www.eff.org/deeplinks/2018/05/attention-pgp-users-new-vulnerabilities-require-you-take-action-now" target="_blank" rel="noopener">release a post saying that there is a potentially serious flaw in the system.</a>

<!--more-->

### Let’s Clear Up Some FUD

At the moment, information on this flaw is scarce, so as always it’s best to wait until the paper is published (which EFF says will be done at 07:00 UTC on May 15th). However, there are some things we can clear up from the start.

Firstly, it does not look like this is a flaw with PGP itself, but rather with implementations of GPG used in email programs such as Enigmail. Both GPG and PGP remain cryptographically sound from what is being said by security experts, so there’s no need to worry that the sky is falling down. The official GPG Twitter account tweeted the following message:

<blockquote class="twitter-tweet" data-lang="en" data-conversation="none">
  <p dir="ltr" lang="en">
    They figured out mail clients which don&#8217;t properly check for decryption errors and also follow links in HTML mails. So the vulnerability is in the mail clients and not in the protocols. In fact OpenPGP is immune if used correctly while S/MIME has no deployed mitigation.
  </p>
  
  <p>
    — GNU Privacy Guard (@gnupg) <a href="https://twitter.com/gnupg/status/995931083584757760?ref_src=twsrc%5Etfw">May 14, 2018</a>
  </p>
</blockquote>

But as always, it is best to wait for the whitepaper to be released.

Secondly, it’s getting rather irksome that researchers are announcing these vulnerabilities without actually giving the public enough information to work with. EFF, TheHackerNews, and the researchers who broke the story are sending people into a panic with a seemingly hyperbolic description of the issue. Details will be released tomorrow, they say, so why not wait until tomorrow to break the story? Sure, they tell people to stop using email plugins for GPG and switch to Signal, but if this <a href="https://lists.gnupg.org/pipermail/gnupg-users/2018-May/060315.html" target="_blank" rel="noopener">thread on the GPG mailing list</a> is to be believed, there really is no need to do so if users stop using HTML emails or follow precautions such as using authenticated encryption and a MIME parser. This sort of information is much better than telling everybody to cease using the product because everything they’ve ever done with it is fundamentally broken.

### The Real Issue Here

Let’s face it: PGP email sucks. From the information available today, it looks as though the issues are mostly being caused by insane defaults in email plugins, or by a user’s misuse of the system (for which there should be no room by design). As I say at the beginning of this post, there are still vestigial strongholds of PGP emails in some industries, but if this whole debacle is any indication of how things are going with it, clearly these systems are too difficult for the average user to use properly.

CSO Online is keeping up-to-date with the issue on their blog, and have given a far better breakdown of  the issue than I can. At this point, all we can do is wait for tomorrow and see what the responsible parties have to say on the matter. I would say, however, that this disclosure has been handled atrociously. There has already been far too much panic over something which has an unknown impact, and perhaps has <a href="https://twitter.com/robertjhansen/status/995929684750815233" target="_blank" rel="noopener">already been mitigated in some circumstances.</a> The researchers <a href="https://twitter.com/seecurity/status/995936859980222464" target="_blank" rel="noopener">seem keen to keep the matter under wraps until the release of the whitepaper</a>, so it will be interesting to see what has been missed out come tomorrow.

### <img style="margin-right: auto; margin-left: auto; float: none; display: block; background-image: none;" title="Don't_Panic.svg" src="https://rootkey.co.uk/wp-content/uploads/2018/05/Dont_Panic.svg_thumb.png" alt="Don't_Panic.svg" width="244" height="145" border="0" />**Update**

[The whitepaper and further details have now been published.](https://efail.de/) As we all know, it&#8217;s not a vulnerability until it has a website a cute logo.

Security researcher Matthew Green has started a thread on Twitter detailling some of his thoughts on the attack. [It&#8217;s very much worth a read](https://twitter.com/matthew_d_green/status/995989254143606789) and he does an excellent job of breaking down the vulnerability into layman&#8217;s terms. Essentially, it looks as though the vulnerability allows attackers to modify encrypted email and add malicious HTML code to it, which is then executed on the receiver&#8217;s computer, allowing the text of the email to be sent to remote servers. This is quite a glaring problem, and [seems to be on the part of the email plugins not detecting the attack and failing to act on it](https://twitter.com/VessOnSecurity/status/995993446283382784) rather than the protocol itself being broken.

The thread goes on to elaborate that PGP is less an issue here than S/MIME, a more widely used protocol which is also vulnerable to attack. This should really have been the meat of the story, as PGP is largely used by a small subset of people while S/MIME is trusted in a lot of critical environments.

This whole (dreadfully handled) debacle needs to be taken as a wake-up call to the fact that **email is an inherently insecure and outdated technology**. People scoff when told to use E2EE messengers, but the fact remains that these apps are made for the modern world and are not stuck in the past in the same way as email is. GPG email is an incredibly old idea that has been shoehorned in to desktop mail programs (which themselves have questionable security when compared to their web counterparts), and is one that really needs to be addressed. Email is more important today than perhaps ever before: it&#8217;s your digital home address, your online home. There needs to be a conversation about increasing security without trying to use overly complex systems such as PGP. And no, [making a Tutanota-styled encrypted message for Gmail is not a solution.](https://www.theregister.co.uk/2018/04/16/google_gmail_security/)

Until the least technically competent among us can use encryption fluently and without frustration, there will be no security at all. Signal and iMessage have both managed to address this problem expertly, providing strong encryption without letting the user see any of the underlying complexity. We have to remember that the average user of technology is [stupid](https://www.over-yonder.net/~fullermd/rants/winstupid/1), and that it is on developers and technology experts to make sure they are protected without having to jump through ridiculous hoops.
