---
id: 137
title: Back to the Drawing Board
date: 2018-06-16T15:22:08+00:00
author: Ciarán Ainsworth
type: post
guid: https://rootkey.co.uk/?p=137
permalink: /2018/06/16/back-to-the-drawing-board/
categories:
  - Technology
tags:
  - active directory
  - ad
  - azure
  - azure active directory
  - ccm
  - centos
  - config manager
  - configuration manager
  - gnu
  - gnu/linux
  - intune
  - linux
  - moodle
  - sccm
  - windows
  - windows 10
---
<figure class="wp-block-image alignleft is-resized"><img src="https://rootkey.co.uk/wp-content/uploads/2018/05/Term2.png" alt="" class="wp-image-56" width="250" height="250" srcset="https://rootkey.co.uk/wp-content/uploads/2018/05/Term2.png 250w, https://rootkey.co.uk/wp-content/uploads/2018/05/Term2-150x150.png 150w" sizes="(max-width: 250px) 100vw, 250px" /></figure> 

<p class="has-drop-cap">
  For the past couple of weeks I&#8217;ve been trying out new things. New to me, anyway. And as you might expect, I&#8217;m really bad at them. Let&#8217;s talk about that!
</p>

<!--more-->

### Why Did it Have to be Moodle?

I work in a very Microsoft-centric environment. The vast majority of our servers are Windows 2016, 2012 R2, and a few straggling 2008 R2 boxes which just refuse to die. Possibly for this reason, we&#8217;ve never had a GNU/Linux technician. Our head of IT is very Linux-literate, but also time-poor. When I moved from the frontline team to the systems team, then, all the Linux responsibilities immediately fell to me.

Fan-fucking-tastic.

My first job with GNU/Linux was to automate patching for the servers and EPOS systems as they hadn&#8217;t been patched in years. That was all simple enough, just write out a nice little cron job with logging and leave them to it. Much less hands-on than trying to manage WSUS. But of all these servers the two most problematic ones are definitely our CentOS boxes which power Moodle.

I had no experience with Moodle until this week, but I had some experience with CentOS. Enough to know that these boxes had been set up by a monkey with absolutely no knowledge of how to partition disk space, set up Apache, or do pretty much anything with GNU/Linux. So when it came time to upgrade Moodle, I was pretty confident that we were going to be boned. 

Luckily, we managed to get the upgrade on the test server finished (not without some trial and error), but much of the way it was set up left myself and our Moodle dev scratching our heads with a &#8220;why the fuck did they do that?&#8221; kind of confuzzlement. Hopefully, this experiment will translate well when we do the live upgrade in a few months, and then we&#8217;ll be able to design the whole server-side from scratch and push it into Azure with proper load-balancing.

### Speaking of Azure

So in [this post](https://rootkey.co.uk/2018/06/02/some-more-windows-work/) I detailed some of the trouble we&#8217;ve been having with getting devices into AAD and Intune management using an SCCM build sequence. I have a quick update to that as well. When last we looked at it we had got devices auto-enrolling in AAD but had yet to hand authority to Intune. When we eventually got the auto enrolment sorted (turns out you have to assign authority to the user group in O365 admin panel, not Azure panel. Go figure) we were still having issues managing the devices in any real way. Intune was simply saying &#8220;MDM status: see configmgr&#8221;. Hum.

My co-worker quickly realised that the SMSTSPostAction I&#8217;d set up was working in that it removed the CCM client from the machine, but it wasn&#8217;t fully successful in turning off EVERY element of CCM&#8217;s authority. While you&#8217;d have thought that uninstalling the client would take authority away, it turns out that it really just leaves it in a managed state as though waiting for CCM to gain control again. Not ideal. With a bit of research my colleague figured out which regkey to turn off and I started writing the new script.

This was a bit more involved than running a simple command line like before. First we needed to change the SMSTSPostAction to call on the PowerShell script from a local directory in bypass mode. Then, as the last step, we needed to map a drive to point to the script&#8217;s location and use another PowerShell script to copy the CCM removal script to the C:\Windows\Temp directory to be executed post OSD completion. The script itself goes like this:

<pre class="wp-block-code"><code>Start-Process -FilePath 'C:\Windows\ccmsetup\ccmsetup.exe' -Args "/uninstall" -Wait -NoNewWindow

# wait for exit
$CCMProcess = Get-Process ccmsetup -ErrorAction SilentlyContinue
try{
    $CCMProcess.WaitForExit()
}catch{

}

# Stop Services
Stop-Service -Name ccmsetup -Force -ErrorAction SilentlyContinue
Stop-Service -Name CcmExec -Force -ErrorAction SilentlyContinue
Stop-Service -Name smstsmgr -Force -ErrorAction SilentlyContinue
Stop-Service -Name CmRcService -Force -ErrorAction SilentlyContinue

# wait for exit
$CCMProcess = Get-Process ccmexec -ErrorAction SilentlyContinue
try{
    $CCMProcess.WaitForExit()
}catch{

}

# Remove WMI Namespaces
Get-WmiObject -Query "SELECT * FROM __Namespace WHERE Name='ccm'" -Namespace root | Remove-WmiObject
Get-WmiObject -Query "SELECT * FROM __Namespace WHERE Name='sms'" -Namespace root\cimv2 | Remove-WmiObject

# Remove Services from Registry
$MyPath = “HKLM:\SYSTEM\CurrentControlSet\Services”
Remove-Item -Path $MyPath\CCMSetup -Force -Recurse -ErrorAction SilentlyContinue
Remove-Item -Path $MyPath\CcmExec -Force -Recurse -ErrorAction SilentlyContinue
Remove-Item -Path $MyPath\smstsmgr -Force -Recurse -ErrorAction SilentlyContinue
Remove-Item -Path $MyPath\CmRcService -Force -Recurse -ErrorAction SilentlyContinue

# Remove SCCM Client from Registry
$MyPath = “HKLM:\SOFTWARE\Microsoft”
Remove-Item -Path $MyPath\CCM -Force -Recurse -ErrorAction SilentlyContinue
Remove-Item -Path $MyPath\CCMSetup -Force -Recurse -ErrorAction SilentlyContinue
Remove-Item -Path $MyPath\SMS -Force -Recurse -ErrorAction SilentlyContinue

# Remove Folders and Files
$MyPath = $env:WinDir
Remove-Item -Path $MyPath\CCM -Force -Recurse -ErrorAction SilentlyContinue
Remove-Item -Path $MyPath\ccmsetup -Force -Recurse -ErrorAction SilentlyContinue
Remove-Item -Path $MyPath\ccmcache -Force -Recurse -ErrorAction SilentlyContinue
Remove-Item -Path $MyPath\SMSCFG.ini -Force -ErrorAction SilentlyContinue
Remove-Item -Path $MyPath\SMS*.mif -Force -ErrorAction SilentlyContinue	
Remove-Item -Path $MyPath\SMS*.mif -Force -ErrorAction SilentlyContinue	

#Remove authority from CCM
$MyPath = “HKLM:\SOFTWARE\Microsoft”
Remove-Item -Path $MyPath\DeviceManageabilityCSP -Force -Recurse -ErrorAction SilentlyContinue</code></pre>

A little lengthy compared to the last one, but lo and behold Intune picked up the new machine and started applying policies. Huzzah!
