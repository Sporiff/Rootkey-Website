================
Personal Matters
================

This post is more of a rant than anything technical. In fact, it's more of a moan born of recent frustrations. 
Nevertheless, this is my website. Where better to moan?

I've recently started a new job working for a software company. My day-to-day work mostly includes rifling 
through SQL databases and making changes as well as searching for causes of issues faced by customers. This 
job has me using a work-issued MacBook Air and a copy of Windows 10 running in a virtual machine. It's the 
setup used by every employee of the company bar one tester, who gets to use GNU/Linux.

I hate this.

I am a particular person. My home PC runs `Arch Linux <https://archlinux.org>`_ with 
`LARBS <lukesmith.xyz/larbs>`_ as a meta-distribution which I have further customized to my liking. I am 
very happy with my setup. It allows me to zip around with comfortable and functional keybindings and to spend 
less time *trying* to do something and more time actually *doing* the thing. This is my machine. This is my 
workflow. macOS has a very different workflow with neither works nor flows in my opinion, but it's a work machine 
so it's really just a minor gripe.

The other day, my partner asked whether she could use my computer to check on something. I said "of course" and 
launched a web browser for her. Within a couple of seconds I registered the confusion, then annoyance, and then 
anger in her face as she tried to grapple with the unfamiliar system. When I offered to look it up for her, she 
just said "why don't you just get a proper computer?"

Now, she has her own laptop which runs Windows 10, though I am yet to actually see her use it for anything. That 
is her computer and it works in a way that she finds at the very least familiar. This comment, however, made me 
realise something: she, like so many others, has never used a personal computer.

What I mean is this: when a person installs an operating system like Windows, macOS, or virtually any distribution 
of GNU/Linux, they will pretty much stick with the system exactly as it is set up. Rather than fix things they don't 
like, they will simply get used to them and then complain when they change. I know because that's exactly how I am 
most of the time. It wasn't until very recently that I started thinking about exactly what it is I need a computer 
to do that I realised I was going about it all wrong by using default interfaces. I needed to address what wasn't 
working for me and learn how to rectify it.

A Short Journey
---------------

I became inspired to start looking into my workflow after reading a book called 
`Clean Code <https://www.oreilly.com/library/view/clean-code/9780136083238/>`_ by Robert C. Martin. In particular, 
the assertion that good code makes it look as though the language was made explicitly for the creation of the 
programme stuck with me. When I look at my work Mac (upon which I am currently writing) I dont feel like it was 
*made* for me. I don't feel like Windows was *made* for me. I honestly don't feel like any one distro of GNU/Linux 
was made for me, either. I just used them because I didn't like macOS or Windows.

The first step I took was to address exactly what was wrong. My MacBook gave me my first hint at some of the issues 
I needed to address in my daily life: 

1. Things on computers are unnecessarily slow. Transitions between workspaces on macOS are frustratingly sluggish. 
Mouse navigation is pointlessly inaccurate and tedious. I need to get rid of the mouse and any annoying transitions. 
OK. But when I started looking around for ways to disable these obnoxiously slow animations, my results were threadbare 
and the result was just a way to "reduce motion sickness" [#f1]_ and so the speed issue remains.

2. Keyboard control on both macOS and Windows is pitiful. So much focus is put on the mouse that navigating the OS without it, 
while possible, is like fighting a honey badger in a pool of molasses. I need to have more control with the keyboard because I 
find mouse navigation annoying.

3. macOS Mojave introduced a dark mode recently, as did Windows 10. However, both of these things are useless if the 
stock apps used are still light and unable to be changed by system-level themes. Web browsers and office programs are 
particularly at fault here. So I need to do away with both wherever possible.

This realisation led me to ricing boards and eventually to `Luke Smith <https://lukesmith.xyz>`_, whose 
`video demonstrating him shaving a few milliseconds off a BASH script <https://www.youtube.com/watch?v=bkgeFi4PwOg>`_ 
greatly impressed me. His setup looked largely ideal, and since he is kind enough to distribute it, I put Arch back 
on my machine and gave it a shot. After a few hours with the manual I was able to go through everything and make the 
changes that made it feel more sensible to me. The whole process took me just one evening with the right base and mindset.

Why Does This Matter?
---------------------

So back to the point at hand. My setup is the result of wanting my computer to do better for me than a default OS 
setup will do. Given the opportunity to sit down and simply *look* at what I was doing with a computer I now have 
a setup that allows me to spend less time on the computer simply because I have got everything done much faster 
than I would have done. Can other people use my computer? Sure, but they would have to work how I work for it to make sense.

Computers are, at this point in time, being produced for all people to use [#f2]_ and as such are tailored to nobody. 
Worse yet, systems like Windows and macOS really don't give you the tools you need to make your experience better. 
It's a bit like walking into a hardware store looking for a screwdriver and all they have is hammers. Sure, you can 
get the work done, but it will be inefficient and ugly. But imagine if you were given a toolbox from which you could 
pick. Given a couple of hours of work and reading, you can wrangle BSD and GNU/Linux to be just that.

I would encourage more people to have *personal* computers. I think the idea of each computer being set up specifically 
for its user while all software works interoperably so that ideas can still be communicated is a wonderful notion. 
What we have at the moment is total homogeny and a societal contempt for anybody who defies it. Mac users are the 
worst of the bunch in this regard, accepting Apple's vision of workflow unflinchingly and unquestioningly. I should 
know, I was/am one by necessity. But Apple's workflow is horrendous, designed to keep you *on* the computer rather 
than allow you to complete work *with* the computer. The distinction is important and becomes abundantly clear once 
you move yourself over to a system which you have tailored to your needs.

If a computer works exactly how you expect it to, ask yourself why you expect it to work that way. Is it because 
it's what makes the most sense to you, or is it because it's simply what has been presented to you before? Think 
about what frustrates you while you use it and how you might address those problems. You may be surprised at what 
you come up with.

.. rubrick:: Footnoes

[#f1]: By the by, Apple, you have utterly failed at design if your system is known to cause motion sickness by default.
[#f2]: Although I'm sure many people with disabilities would disagree
