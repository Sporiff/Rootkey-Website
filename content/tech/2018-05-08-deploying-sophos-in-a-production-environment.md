---
id: 61
title: Deploying Sophos in a Production Environment
date: 2018-05-08T22:03:02+00:00
author: Ciarán Ainsworth
layout: post
guid: https://rootkey.co.uk/?p=61
permalink: /2018/05/08/deploying-sophos-in-a-production-environment/
categories:
  - Technology
tags:
  - antivirus
  - av
  - central
  - cloud
  - Sophos
---
<img class="alignnone size-medium wp-image-66 aligncenter" src="https://rootkey.co.uk/wp-content/uploads/2018/05/anti-virus-300x200.jpg" alt="" width="300" height="200" srcset="https://rootkey.co.uk/wp-content/uploads/2018/05/anti-virus-300x200.jpg 300w, https://rootkey.co.uk/wp-content/uploads/2018/05/anti-virus-768x511.jpg 768w, https://rootkey.co.uk/wp-content/uploads/2018/05/anti-virus-1024x682.jpg 1024w, https://rootkey.co.uk/wp-content/uploads/2018/05/anti-virus.jpg 1200w" sizes="(max-width: 300px) 100vw, 300px" />

AntiVirus is a necessary evil. With the world more connected than ever before, every device needs protection and tools to allow administrators oversight. Currently, the AV I work with is Sophos&#8217; cloud-based solution for Windows, Mac, and Linux. When I was working on the frontline, I quickly became aware that Sophos did not deploy during imaging as it had initially done, nor could it easily be pushed out via SCCM. We spent a long time going around to each freshly imaged machine and loading Sophos on, rebooting the machine, and logging tickets to have its policy applied. This did not sit right with me, so upon my move to the systems team I decided to have a crack at simplifying the process.

<!--more-->

### Build Deployment

The issue, I soon discovered, was not one of packaging or deployment, but rather time. Sophos&#8217; installer goes out of date every 30 days, at which point a new executable needs to be downloaded for any new deployments. Understandably, this was not a practical solution for our application manager. Given my GNU/Linux background, the solution seemed straightforward enough: write a script to handle download and installation as one does with the Linux client. However, while this was easily facilitated by PowerShell&#8217;s Invoke-WebRequest cmdlet on Windows 10, Windows 7&#8217;s older (v2) Powershell had no such functionality. So we were faced with a dilemma: upgrade PowerShell or find a way to script the download and install in an older environment.

The first option seemed the best to both of us, as more PowerShell features for technicians on a machine could be useful in the long run. However, we encountered endless issues trying to deploy the upgrade packages in the OSD, so eventually I sat down to work out the script in .NET. The result was a much less clean but still functional PowerShell script.

Both of these scripts can be found on my [GitHub](https://github.com/Sporiff/Sophos-Install).

### Protecting macOS

macOS is somewhat more familiar to me than Windows due to its UNIX base, but I was not yet used to working with DeployStudio to image macOS devices. I decided that if I could automate deployment for Windows, the same must be easy enough on macOS. Certainly, the download command curl is a lot more familiar to me than Invoke-WebRequest, however the installation of Sophos from a .zip required many more steps than simply executing a .exe like in Windows. More importantly, I came across another issue.

[Sophos had reported](https://community.sophos.com/kb/en-us/131749) a change to their installer which caused it to conflict with the default permissions set by DeployStudio during imaging. For this reason, it was necessary to edit the workflow thus: place the installer script in as a delayed script to run after first boot, then place a chmod script at the end of the sequence to run as the last step of the initial build process. Lo and behold, a working Sophos install.

Both of these scripts can be found on my [GitHub](https://github.com/Sporiff/Sophos-Mac).

### Clearing False Positives

This one may be seen by some as a bit of a nasty hack, but unfortunately Sophos &#8211; like any software &#8211; is imperfect. By default, the AV will quarantine or clean up PUAs and viruses from a host machine and report the status of this operation in the console, usually with a yellow exclamation triangle for &#8220;medium&#8221; status or a red exclamation circle for &#8220;bad&#8221; status. Usually, manually clearing out these problems is sufficient to repair the status, but sometimes the Sophos Health service gets&#8230; well, stuck.

Clearing this one is a little messy, as I say, but it&#8217;s quite simple. First, the technician should verify that the threat has indeed been cleared. Then, tamper protection should be disabled on the affected machine. This is to allow the technician to stop the Sophos Health service, rename the stuck database, and re-enable the service. Tamper protection should then be enabled and the machine reset. Voilà! A nice green tick in Sophos Central.

This script can be found on my [GitHub](https://github.com/Sporiff/Sophos-Health-Fix).

### Going Forward

Working with Sophos is something of an adventure. Due to the fact that it is cloud-based, it is always evolving. Sometimes, the changes are welcome &#8211; like a long-awaited fix for a problem &#8211; but other times they can present new challenges to which you will quickly need to adapt your environment. With a little scripting, Sophos management for endpoints becomes a lot easier.
