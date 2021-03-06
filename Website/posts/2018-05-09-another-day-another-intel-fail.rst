==============================
Another Day Another Intel Fail
==============================

.. post:: May 09, 2018
   :author: Sporiff
   :tags: tech, intel, security, meltdown, spectre
   :category: Tech
   :location: Exeter
   :language: en
   :redirect: tech/2018-05-09-another-day-another-intel-fail

A few years ago, I'd have been shocked to see the sort of vulnerabilities I see announced these days 
once a year, let alone once a month. But the rise of cybersecurity as a respected industry has led to the 
big companies such as Microsoft, Apple, Canonical, etc. pulling up their socks and crushing vulnerabilities 
in a timely manner. This is all great, except for those of us whose job it is to deploy these fixes.

The big news out of this week's Patch Tuesday was undoubtedly the announcement of `yet another flaw with 
Intel's x86 architecture <https://www.theregister.co.uk/2018/05/09/intel_amd_kernel_privilege_escalation_flaws/>`_. 
Following on from the disastrous `Meltdown and Spectre <https://meltdownattack.com/>`_, the latest vulnerability 
allows applications to hijack code and read memory due to a mistake in the implementation of kernel-level code. 
*Nota bene*, I am not a programmer or a chip expert, this is just what I've gleaned from El Reg's far better 
explanation of the issue. What is more interesting to me is that this is not a flaw with x86 *per se*, but rather 
"the error appears to be due to developer interpretation of existing documentation."

Bad Documentation is Bad
------------------------

If an individual reads a set of instructions and manages stumble along the way, that's understandable and 
usually rectifiable. However, if everybody reading the instructions fails in the same way, the instructions 
are bad. End of story. This cannot be passed off as being developer error if the documentation was misleading or 
unclear. It can be presumed that Microsoft, Google, Apple, and companies/communities working on \*BSD and Linux 
are pretty smart cookies indeed. I feel it is unlikely that all of them could have failed in the same way without 
being misled.

So what does this mean? I would personally be taking a good long look at Intel after the disastrous year they've 
been having and wondering whether or not we had made a huge mistake entrusting our standards and architectures to a 
company so driven by quick wins and marketing that they will happily sacrifice security and the long-term health 
of the industry. As their recent "meltdown" has shown us, they have been a company obssessed with being able to 
claim the fastest and bestest at the cost of sane design. And let's face it: `these chips aren't really getting 
that much better <https://www.cnet.com/news/intel-kaby-lake-7th-gen-7700-7600-7350/>`_.

It seems that `Apple's recent decision to move away from Intel architecture <https://www.bloomberg.com/news/articles/2018-04-02/apple-is-said-to-plan-move-from-intel-to-own-mac-chips-from-2020>`_
may not be such a crazy idea. Certainly, Intel is faster *now*, but it is only a matter of time and investment 
before others surpass them. Certainly, they've not made themselves many friends this year. This month's vuln 
is naught more than another eye-roll for sysadmins and users at this point, a fault for which developers are 
being unfairly blamed.

Anything Else?
--------------

Flash Player, of course, has `yet another high vulnerability announcement <https://helpx.adobe.com/security/products/flash-player/apsb18-16.html>`_. 
Time to get your Adobe hat on and grumble all the way to deployment on this one. Or better yet, get rid of it 
entirely and see how many people actually complain. Most of the rest of the serious stuff (17/21 critical) 
seems to be browser vulnerabilities. Par for the course...
