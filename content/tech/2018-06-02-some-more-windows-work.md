---
id: 123
title: Some More Windows Work
date: 2018-06-02T20:14:52+00:00
author: Ciarán Ainsworth
layout: post
guid: https://rootkey.co.uk/?p=123
permalink: /2018/06/02/some-more-windows-work/
categories:
  - Technology
tags:
  - aad
  - active directory
  - ad
  - azure
  - bitlocker
  - config manager
  - configuration manager
  - osd
  - sccm
  - task sequence
  - ts
  - windows
  - windows 10
---
<figure class="wp-block-image alignleft is-resized"><img src="https://rootkey.co.uk/wp-content/uploads/2018/05/Term2.png" alt="" class="wp-image-56" width="259" height="259" srcset="https://rootkey.co.uk/wp-content/uploads/2018/05/Term2.png 250w, https://rootkey.co.uk/wp-content/uploads/2018/05/Term2-150x150.png 150w" sizes="(max-width: 259px) 100vw, 259px" /></figure> 

<p class="has-drop-cap">
  It&#8217;s been a little while since I last posted here. I&#8217;ve been having a very busy and productive time at work. Let&#8217;s jump in to some of the stuff I&#8217;ve learned over the past couple of weeks. Come on! It&#8217;ll be fun!
</p>

Or not. Who knows? More importantly, who cares what you think? It&#8217;s my blog and I WILL cry when I want to.

<!--more-->

### It&#8217;s All About the Cloud

As we all know, Cloud services are the way absolutely everything on God&#8217;s green bloody earth are going these days. Microsoft device management is no different. So, of course, with our upcoming move to Windows 10 we need to devise a way to get all of our mobile devices managed by InTune rather than SCCM and Azure Active Directory rather than on-prem AD just so we can ship them out into the big wide world and still keep them managed. This sounds like it should be easy, but when you&#8217;re working at the scale my job is working at the prospect of joining devices in bulk to Azure cloud services is daunting. Microsoft&#8217;s official recommendation seems to be going to each device in turn and joining them to AAD/InTune using the built-in wizard, but of course we don&#8217;t have the time or staff to do that. Let&#8217;s script it out instead!

### OSD Teething Issues

So you&#8217;ve built your Windows 10 image and got everything set up just how you like it. You insert it into your task sequence, and start applying all your drivers and software. Unfortunately, you&#8217;re trying to use Office 365 Pro Plus with shared user activation and have packaged it as an application as per [Microsoft&#8217;s instructions](https://docs.microsoft.com/en-us/deployoffice/deploy-office-365-proplus-with-system-center-configuration-manager). Well, let me tell you now sonny Jim, that ain&#8217;t gonna work. While the deployment of this application will actually be successful, you are going to see your builds failing with error code 16389 A LOT when trying to run this in the OSD. What is error code 16389? Fuck knows. A lot of Googling brought up nothing useful.

Now you&#8217;re running around like a headless chicken changing every single deployment rule you can think of. When you try installing on a live system, it returns code 0 so clearly it&#8217;s working. Try it again in the OSD, 16389. Utter madness. I spent a good day and a half trying to work out what the hell could be wrong. The detection method was right, the application worked, the content was accessible and upon completing the build the software was present, but the error code caused the sequence to complete with some problems

So how do we get around this? We&#8217;ve used the wizard and packaged everything up nice exactly how M$ recommends but it&#8217;s still not working. Well then. Clearly, Microsoft doesn&#8217;t know what they&#8217;re talking about. Let&#8217;s instead build the O365 PP installer as a traditional package with a script installer. Does it work now? You&#8217;re goddamn right it does. No more software failures!

### Ensuring Authority

So one thing you&#8217;re going to come across that can stump you a little is setting up InTune as the authority for software management on your mobile device. See, when you build a machine using SCCM you load a config manager client on to the machine which deals with setting up software and managing communication to the CCM server. However, the presence of this client will cause InTune to reject the machine as it doesn&#8217;t have the proper authority. Since we&#8217;re not going down the path of co-management (InTune only for mobile devices), we need to get that off of there.

Now, the trick  with this is remembering that getting rid of the CCM client will ALWAYS cause a task sequence failure. If you insert a script as the last step with the command C:\Windows\ccmsetup\ccmsetup.exe /uninstall you&#8217;re going to get a whole load of error messages in your trace log and ultimately the build will fail. This is going to cause oddities. We need a smarter way around this. One way would be to set up a Powershell script in the build which sets a scheduled task to run the command after the machine&#8217;s first boot, but surprisingly there&#8217;s a more elegant way to do it.

If you add a Task Sequence variable during the OS setup with the variable SMSTSPostAction and write the above script in as a command, the machine will store this command to run it upon successful completion of the task sequence. After a bit of testing and tweaking, I started to get successful builds with no CCM client on them at all. All the benefits of the OSD with none of the residuals! Hoorah!

## Azure AD is a Bastard

Okay, so we know that these machines need to join Azure AD and not on-prem AD. This seems like it should be easy enough, but unfortunately Microsoft are not keen on bulk joining devices like this. Eventually, it looks like they caved under the pressure from (justifiably) annoyed enterprise customers and created a WICD tool to enable bulk enrollment using a ppkg.

We used a ppkg to set some of our customization in the build, so we were fairly confident this would work perfectly. Using DISM, we applied both to the image and watched eagerly for our asset to appear in AAD.

It never did.

We saw a device joining, for sure, but it wasn&#8217;t anything we recognised. DESKTOP-£&$\*£&\*&#8221; something or other. Not anything like our standard naming convention. But it was being joined by the bulk join account. Hmm.

So I did a bit of digging around in the registry and found the issue. The machine&#8217;s ORIGINAL name is DESKTOP-whatever. This value gets changed in the TS but only once you boot in to the live image. Of course, using DISM we&#8217;re working with an offline image, so it&#8217;s joining the machine using this old name and then going online. Well that&#8217;s not good enough. We need to find a way to apply the package using an online image. To cut a long story short, here&#8217;s how I did it.

First, we add a step in the TS to map a network drive. We point this to the UNC of the folder on our config manager server where the PPKG is stored and we use a login with the relevant permissions to access it so we can be sure it&#8217;s picking it up. Then, we write the Powershell script to actually install the ppkg:

<pre class="wp-block-code"><code>cd Z:\
Install-ProvisioningPackage -Path "YourPackage.ppkg" -ForceInstall -QuietInstall</code></pre>

Now just package up that script, add a step to run it after mapping the drive. Badda-bing, badda-boom, the machine now joins AAD with the name you assigned in the TS. Hooray!

### Bitlocker

The next step is to ensure that your machine Bitlockers properly and passes the key back to AAD so that your organisation can recover it if necessary. This is easy enough when you have a device like a Surface which supports [InstantGo](https://blogs.technet.microsoft.com/home_is_where_i_lay_my_head/2016/03/14/automatic-bitlocker-on-windows-10-during-azure-ad-join/), but I&#8217;m not lucky enough to just be working with these high-end devices. Some of our devices are kind of shit, yo. So we need a universal solution. Powershell to the rescue once more.

The trick here is the order in which you do things. This Bitlocker step should obviously be placed after the Azure AD Join step we made earlier. The machine will then need to provision a key, back up the key to Azure AD and then encrypt the drive with that key. The script looks like this:

<pre class="wp-block-code"><code>#Provision the key first and force it to encrypt the drive with the provisioned key

Add-BitLockerKeyProtector -MountPoint "C:" -RecoveryPasswordProtector

#Create the variable $BLV with the value of the bitlocker'd C:\ drive

$BLV = Get-BitLockerVolume -MountPoint "C:"

#Back the key up to Azure for the drive that needs to be encrypted, assigning the drive's ID

BackupToAAD-BitLockerKeyProtector -MountPoint "C:" -KeyProtectorId $BLV.KeyProtector[0].KeyProtectorId

#Enable Bitlocker on the system drive (without a login pin)

Enable-BitLocker -MountPoint "C:" -EncryptionMethod XtsAes256 -UsedSpaceOnly -TpmProtector </code></pre>

Package it, add the step, and move on with your life. The machine will now Bitlocker the used space and back the key up to the item in Azure AD.

### InTune Management

This last bit is the bit I still haven&#8217;t got working. As I mentioned before, we&#8217;re trying to give InTune full authority over the devices rather than SCCM, so what we&#8217;ve looked at doing is setting up a group within AAD which have device adding rights. As of yet, the devices are not pulling through to InTune despite being owned by the users in that group. From what I can see, we may not be able to get it working that way. However, my suspicion is that if we use CCM&#8217;s co-management setup during the build but then remove the CCM client as per the steps above, it will enroll the device and remove CCM&#8217;s authority. More testing is required, but hopefully we&#8217;ll have got it working next week.

Phew! It&#8217;s been a bit of a slog, but a very productive one. Hope someone finds this somewhat useful.