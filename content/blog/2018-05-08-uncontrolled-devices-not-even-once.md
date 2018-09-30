---
id: 51
title: Uncontrolled devices. Not even once.
date: 2018-05-08T20:27:24+00:00
author: Ciarán Ainsworth
layout: post
guid: https://rootkey.co.uk/?p=51
permalink: /2018/05/08/uncontrolled-devices-not-even-once/
categories:
  - Technology
tags:
  - alexa
  - amazon
  - tech
  - technology
---
<img class="size-medium wp-image-52 aligncenter" src="https://rootkey.co.uk/wp-content/uploads/2018/05/Hal-300x300.png" alt="" width="300" height="300" srcset="https://rootkey.co.uk/wp-content/uploads/2018/05/Hal-300x300.png 300w, https://rootkey.co.uk/wp-content/uploads/2018/05/Hal-150x150.png 150w, https://rootkey.co.uk/wp-content/uploads/2018/05/Hal.png 720w" sizes="(max-width: 300px) 100vw, 300px" />

<p id="0889" class="graf graf--p graf-after--figure" style="text-align: left;">
  I’m a systems administrator by trade (or as I often find myself writing, a “systemd administrator” since <a class="markup--anchor markup--p-anchor" href="http://without-systemd.org/wiki/index.php/Main_Page" target="_blank" rel="nofollow noopener" data-href="http://without-systemd.org/wiki/index.php/Main_Page">that cancerous piece of bloatware</a> consumes most of my troubleshooting life). It is, therefore, perhaps unsurprising that I like having control over the devices in my home. I refuse to use Apple devices apart from the one I have to use for work, I allow only GNU/Linux devices to be used on our home WiFi, and while my rooted Android phone is still somewhat lacking in end-user control, I plan to replace it with the <a class="markup--anchor markup--p-anchor" href="https://puri.sm/shop/librem-5/" target="_blank" rel="nofollow noopener" data-href="https://puri.sm/shop/librem-5/">Librem 5</a> when it launches.
</p>

<p id="db86" class="graf graf--p graf-after--p graf--trailing" style="text-align: left;">
  It would be terribly naïve of me to presume that other people were so preoccupied with device control as myself. My partner, for example, is the sort of person who simply wants her computer to get out of her way when she’s using it. The slightest hint of maintenance or troubleshooting is enough to make her throw the machine down in anger and find any excuse not to continue with what she was doing. I believe that a lot of people are like this, and it is what has given rise to this wave of “easy tech”, or technology with which the user has little interaction and no ability to troubleshoot. <strong class="markup--strong markup--p-strong">This is a terrible thing</strong>.
</p>

<!--more-->

### No Access? No answers.

This little rant of mine was inspired in part by <a class="markup--anchor markup--p-anchor" href="https://medium.com/@selectall/this-is-why-alexa-is-laughing-at-you-82e536ca595e" target="_blank" rel="noopener" data-href="https://medium.com/@selectall/this-is-why-alexa-is-laughing-at-you-82e536ca595e">this article</a> about Amazon’s Alexa and her recent spout of the giggles. For those of you who are not members and therefore cannot get past the paywalls, here is the part that piqued my interest:

<p style="text-align: center;">
  <a class="markup--anchor markup--pullquote-anchor" href="https://medium.com/@selectall/this-is-why-alexa-is-laughing-at-you-82e536ca595e" target="_blank" rel="noopener" data-href="https://medium.com/@selectall/this-is-why-alexa-is-laughing-at-you-82e536ca595e">You’ll just have to take [Amazon’s] explanation of misheard commands at face value: None of the processing is done client-side, there is no way for third parties to look at how Alexa devices really work, to poke around in the guts and discover causes and effects. ~ Brian Feldman</a>
</p>

<p id="da9c" class="graf graf--p graf-after--pullquote">
  End users seem to be infatuated with devices like Alexa and Google’s Home devices. My aunt has not only got one for herself but has also inflicted one on my wholly disinterested grandmother. Quite literally, everybody and their grandma seems to have one. It’s become one of those “must-have” gadgets for which everybody clambers on Cyber Monday and Black Friday and you can see why: it allows users to interact with services they already know and love but with an even greater degree of laziness than a laptop or smartphone already affords.
</p>

<p id="18c0" class="graf graf--p graf-after--p">
  There’s something very sci-fi about the idea of walking into your home and being able to turn things on and off with a simple command, about being able to talk to your very own robot butler about today’s headlines and weather. People are excited by future things, and that is why Amazon’s Alexa and her ability to order you food with a simple grunt is so appealing to so many.
</p>

<p id="2fd8" class="graf graf--p graf-after--p" style="text-align: left;">
  But Alexa and her ilk are also sci-fi in a much darker way. I’m not talking about the fact that they suck up data like a sentient drug-addicted vacuum snorts coke at a 70s night. I’m not even talking about the notion that these devices might somehow become intelligent and launch a Skynet-style attack on humanity. My problem is far more practical: I cannot fix it when it breaks.
</p>

<p style="text-align: center;">
  <a class="markup--anchor markup--pullquote-anchor" href="https://medium.com/@selectall/this-is-why-alexa-is-laughing-at-you-82e536ca595e" target="_blank" rel="noopener" data-href="https://medium.com/@selectall/this-is-why-alexa-is-laughing-at-you-82e536ca595e">Amazon produced a statement claiming that the problem was the Echo devices incorrectly hearing themselves being commanded to laugh. “We are disabling the short utterance ‘Alexa, laugh.’ We are also changing Alexa’s response from simply laughter to ‘Sure, I can laugh’ followed by laughter,” the company said in a statement. ~ Brian Feldman</a>
</p>

<p id="1b08" class="graf graf--p graf-after--pullquote">
  Amazon’s reported fix for the problem seems reasonable enough. Alexa is simply mishearing you, so we’ll change that command and make it so that she issues a warning when she’s about to laugh like a child in an 80s horror movie. For most users, this will satisfy. But I have to ask the question: how do <em class="markup--em markup--p-em">I </em>know that this is what the problem was? We now know that Alexa was reportedly mishearing commands such as “lamp” and “light” as “laugh”. And if my command history did not include any of these, or I simply did not have any connected devices like these, and it still laughed then how could I go about finding the solution?
</p>

<p id="f718" class="graf graf--p graf-after--p">
  When my computers break (which they frequently do, usually due to my actions) I have a fairly good shot at finding out what is wrong. Log files are easily accessed and read, Googling around usually brings me some sort of solution. At the end of the day, even on a Windows machine I can typically find some sort of log which will tell me exactly what happened just before the whole system fell over. Even if it’s a stupid piece of software, something will get logged. But with this new range of devices, so imprisoned by the companies which manufacture them, access is getting more and more limited for the end-user.
</p>

<p id="6715" class="graf graf--p graf-after--p">
  Take, as an example, the iPhone. While Android definitely dominates in terms of numbers, iOS remains the gold standard for many. Now, I use one of these accursed things for work and it goes wrong <strong class="markup--strong markup--p-strong">constantly</strong>. Usually, it’s something simple like WiFi dropping off every time I lock the phone then taking around 30 seconds to reconnect. If it were an Android, I could look in the system logs to find out why this was happening, but with iOS devices, I would need to own a Mac to do this. Vendor lock-in is becoming a severe issue, and devices like Google Home and Amazon Alexa, which do all of their processing server-side, present an even greater level of vendor-control and user-cuckoldry than ever before.
</p>

### Insist on control {#2430.graf.graf--h3.graf-after--p}

<p id="89c1" class="graf graf--p graf-after--h3">
  As I said before, I know many people for whom the idea of having more involvement with their computers and the maintenance of them is a horrifying thought. After all, computers are complicated and confusing, so the less you have to do with them the better, right? Absolutely not. Here’s my unpopular opinion upon which I’m certain I will expand at a later date: <strong class="markup--strong markup--p-strong">computers should be hard to use. </strong>These devices should require some investment of time to come to grips with, just like using any power tool should require the wearing of correct safety equipment and following best practices. If <a class="markup--anchor markup--p-anchor" href="https://www.over-yonder.net/~fullermd/rants/winstupid/1" target="_blank" rel="nofollow noopener" data-href="https://www.over-yonder.net/~fullermd/rants/winstupid/1">users are stupid,</a> we are going to be in a whole heap of trouble as computers start to take over our lives more and more.
</p>

<p id="fc4e" class="graf graf--p graf-after--p">
  Home computers and smartphones are immensely powerful machines with which an individual can potentially do a great deal, but unfortunately most people have given away all control in favour of having a simpler time. But this attitude is incredibly wasteful. If you spend a certain amount of money on a device, you should get your money’s worth from it. Similarly, if you’re using any device for anything personal or professional, the only person who should be controlling it and interacting with your data <strong class="markup--strong markup--p-strong">is you.</strong>
</p>

<p id="0e4b" class="graf graf--p graf-after--p">
  If you have no idea how a machine works, and you have no way of fixing a problem or even finding out what the problem is or relates to, then you have wasted your money. You have bought yourself an expensive trash can and, in the end, it doesn’t even really belong to you. Sure, it sits in your house and you <em class="markup--em markup--p-em">think </em>it’s doing what you ask it to, but in reality you don’t know what its capabilities are nor do you know what its limitations are. That is a very sad fact.
</p><figure style="width: 402px" class="wp-caption aligncenter">

<img class="progressiveMedia-image js-progressiveMedia-image" src="https://cdn-images-1.medium.com/max/800/1*cjAXyYTc8oCD4EkSIGcQgw.jpeg" alt="" width="402" height="599" data-src="https://cdn-images-1.medium.com/max/800/1*cjAXyYTc8oCD4EkSIGcQgw.jpeg" /><figcaption class="wp-caption-text">Amazon Alexa 2.0</figcaption></figure>