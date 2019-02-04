---
id: 140
title: 'Microsoft Teams: Deployment Headaches'
date: 2018-07-14T22:00:34+00:00
author: Ciarán Ainsworth
type: post
guid: https://rootkey.co.uk/?p=140
permalink: /2018/07/14/microsoft-teams-deployment-headaches/
categories:
  - Technology
tags:
  - admx
  - deployment
  - gpo
  - group policy
  - installer
  - intune
  - microsoft
  - msi
  - office 365
  - sccm
  - teams
---
<figure class="wp-block-image alignleft"><img src="https://rootkey.co.uk/wp-content/uploads/2018/05/Term2.png" alt="" class="wp-image-56" srcset="https://rootkey.co.uk/wp-content/uploads/2018/05/Term2.png 250w, https://rootkey.co.uk/wp-content/uploads/2018/05/Term2-150x150.png 150w" sizes="(max-width: 250px) 100vw, 250px" /></figure> 

<p class="has-drop-cap">
  So you want to use Microsoft Teams in your organisation, huh? Are you prepared for the pain? The lack of documentation? The feeling of utter exasperation at a company unable to properly consider the needs of enterprise customers in their new &#8220;modern&#8221; approach? Well, I&#8217;m here to share my experiences with you so that hopefully you can avoid some of the pain I had to go through in getting this all ready for our upcoming rollout.
</p>

<!--more-->

### SCCM Deployments

Like many of you, my organisation utilises Microsoft&#8217;s powerhouse deployment tool: SCCM. Initially, when Teams was released it was only available as an executable, which is fine. A little jiggery-pokery with detection methods yielded a usable application which could easily be deployed and self-served by users. Unfortunately, deploying the .exe and then trying to switch to the .msi requires the use of a [cleanup script](https://docs.microsoft.com/en-us/MicrosoftTeams/scripts/powershell-script-teams-deployment-clean-up), so we were lucky that Microsoft &#8211; in their infinite wisdom &#8211; decided to release [the .msi &#8220;machine-wide installer&#8221;](https://docs.microsoft.com/en-us/MicrosoftTeams/msi-deployment) before we started deploying. Saves us a bit of work.

The .msi installer, as you might imagine, works perfectly as an application. The only caveate is that the installer is per-user no matter what you specify in your application settings. The program installs to the users&#8217; AppData folder, so it can cause headaches in environments which make use of redirected AppData.

### Intune Deployments

This is where things got a lot more complicated. Intune is at once a product with a great deal of documentation and one with absolutely no useful information. So when my co-worker and I initially uploaded the .msi to Intune we expected it to&#8230; work. Nobody else had complained of any issues with the deployment via Intune, so we were surprised when every attempt at installation came back as having failed.

So here&#8217;s the thing: Intune HATES deployments based on devices. Nearly every device configuration profile, compliance profile, and application deployment is expected to be done per-user. This does not fit our use-case, as users need to interact with many different devices in different contexts (e.g. shared user devices and personal devices). For that reason, we&#8217;ve had a lot of problems getting policies to work and are currently trying to get M$ to fix the issue.

But what about Teams? After all, the Chrome Enterprise installer worked fine targeting devices. The error message from Intune was that devices could not be targeted with user-based installers. But Teams is a &#8220;machine-wide installer&#8221;, right? Well, no. Not exactly. The installer basically just sets up a base downloader that is initiated by a user logging in. So how do you get Intune to recognise it as a device installer? You edit the .msi of course.

I personally use [SuperOrca](http://www.pantaray.com/msi_super_orca.html) to do this, but there are other tools available which can do the same job. Basically you just load up the .msi and add the following value to a new row:

<pre class="wp-block-code"><code>Property:
ALLUSERS=2</code></pre><figure class="wp-block-image aligncenter">

<img src="https://rootkey.co.uk/wp-content/uploads/2018/07/Teams.png" alt="" class="wp-image-141" srcset="https://rootkey.co.uk/wp-content/uploads/2018/07/Teams.png 509w, https://rootkey.co.uk/wp-content/uploads/2018/07/Teams-300x231.png 300w" sizes="(max-width: 509px) 100vw, 509px" /></figure> 

This will set the default installation target to &#8220;machine&#8221;, and will enable deployment of the .msi to device collections within Intune.

### Stopping AutoStart

The single most annoying thing about Microsoft Teams is its inability to shut up. By default, the application is set to launch in the foreground at login. It&#8217;s not a small app, either, so on older machines it increases load up time substantially. When you&#8217;re trying to get non-techie users to use new tech (teachers, for example) the LAST thing you want to do is annoy them with clunky, slow applications.

So, I started poring over the documentation looking for an enterprise kill switch for this behaviour. An ADMX template, maybe? Doesn&#8217;t look like it. A native GPO for Teams behaviour? Nope. Hmm. Okay then. Let&#8217;s brush off [Procmon](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) and look at what this is doing.

So it looks like when you enable/disable the autostart from within the Teams desktop application it writes and verifies a bunch of regkeys. The one we&#8217;re interested in is this one:

<pre class="wp-block-code"><code>HKCU\Software\Microsoft\Windows\CurrentVersion\Run
com.squirrel.Teams.Teams
C:\Users\%currentuser%\AppData\Local\Microsoft\Teams\Update.exe --processStart "Teams.exe" --process-start-args "--system-initiated"</code></pre>

So, by creating a login script using group policy which searches for and removes this key, we effectively wipe out the autostart behaviour. Now, we have a working Teams deployment on SCCM. As for Intune, it remains to be seen whether or not creating a CSP and installing local GPOs will consistently suppress autostart. To be tested next week.
