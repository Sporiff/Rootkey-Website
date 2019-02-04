---
id: 145
title: Deploying and Controlling Google Chrome Settings Using Microsoft Intune
date: 2018-07-27T10:19:38+00:00
author: Ciarán Ainsworth
layout: post
guid: https://rootkey.co.uk/?p=145
permalink: /2018/07/27/deploying-and-controlling-google-chrome-settings-using-microsoft-intune/
categories:
  - Technology
tags:
  - admx
  - chrome
  - google
  - gpo
  - homepage
  - intune
  - microsoft
---
<figure class="wp-block-image alignleft"><img src="/wp-content/uploads/2018/05/Term2.png" alt="" class="wp-image-56" srcset="/wp-content/uploads/2018/05/Term2.png 250w, /wp-content/uploads/2018/05/Term2-150x150.png 150w" sizes="(max-width: 250px) 100vw, 250px" /></figure>

<p class="has-drop-cap">
  Righto. This one has given me a mild headache for the last couple of days, but I&#8217;ve found a workable solution that allows me to set a home page for users in Chrome. You would have thought that would be really easy, right? Well, Google in its infinite wisdom has decided that conventional Windows management is for wusses. So down the rabbit hole we go.
</p>

### Ingesting ADMX Templates

Let us say, for the sake of argument, that you have deployed the Chrome Enterprise .msi to your devices either during a build or using Intune. Now you want to control some of the settings for your users in order to provide a consistent experience. If you look around online, you&#8217;ll see various references to ingesting ADMX templates and leveraging these for using with Chrome. This looks something like this:

  * Create an OMA-URI to import your ADMX template
      * ./Device/Vendor/MSFT/Policy/ConfigOperations/ADMXInstall/Chrome/Policy/ChromeADMX}
      * Data Type = String
      * Value = the whole body of the Chrome ADMX template
  * Leverage policies from within this template using nested OMA-URI settings
      * ./Device/Vendor/MSFT/Policy/Config/Chrome~Policy~googlechrome/ShowHomeButton
      * Data Type = String
      * Value = <enabled/>

Pretty simple stuff. This allows us to show the home button. Tickedy boo. So what about setting the home page and landing pages for users? Well, unfortunately, this isn&#8217;t possible.

When you use the CSP settings to load and edit the GPO locally, Chrome detects that the computer is not connected to an on-prem AD, and will ignore certain settings including but not limited to your custom home page. Bum gravy. This is mentioned in some capacity in [this thread](https://bugs.chromium.org/p/chromium/issues/detail?id=433112) and in [this advisory](https://www.chromium.org/administrators/policy-list-3#HomepageLocation), but it&#8217;s far from a satisfactory explanation in an Azure AD connected world.

### What is a Boy to do?

So here&#8217;s the dilemma. I know that another way to control Chrome is using a file called &#8220;master_preferences&#8221; located in the C:\Program Files (x86)\Google\Chrome\Application folder, so for future deployments it would be entirely possible to simply place this in the image. However, we have nearly 60 devices already out in the world due to the unreal pressure we&#8217;ve been under to deliver, so I need a remote deployment method.

First option is to use Powershell. Powershell could easily be pointed at a publicly accessible Azure blob containing the master_preferences file. The problem with this is that Powershell scripts can only be targeted at users, which would mean that in the long run we would not be able to set different preferences on different types of machines.

So, what about a .msi file? I got this idea in my head to package the file up as an application, which would give us the power not only to deploy per device but also to supersede if we ever want to make changes to the config. The two tools I used to achieve this were [NSIS](http://nsis.sourceforge.net/Download) and the free version of [exemsi](https://www.exemsi.com/download/). The steps are as follows:

  * Edit your master_preferences file to reflect the changes you want to make
  * Place the file in a zip archive
  * Run NSIS and select &#8220;Installer based on ZIP file&#8221;<figure class="wp-block-image aligncenter">

<img src="/wp-content/uploads/2018/07/image.png" alt="" class="wp-image-146" srcset="/wp-content/uploads/2018/07/image.png 604w, /wp-content/uploads/2018/07/image-300x195.png 300w" sizes="(max-width: 604px) 100vw, 604px" /></figure>

  * Select your ZIP file, give the installer a suitable name, and set the &#8220;Default Folder&#8221; value to &#8220;C:\Program Files (x86)\Google\Chrome\Application&#8221;<figure class="wp-block-image aligncenter">

<img src="/wp-content/uploads/2018/07/image-1.png" alt="" class="wp-image-147" srcset="/wp-content/uploads/2018/07/image-1.png 546w, /wp-content/uploads/2018/07/image-1-300x257.png 300w" sizes="(max-width: 546px) 100vw, 546px" /></figure>

  * Hit &#8220;Generate&#8221; and locate your newly created .exe file
  * Next, open up exemsi and click &#8220;Next&#8221; to start the wizard<figure class="wp-block-image aligncenter">

<img src="/wp-content/uploads/2018/07/image-2.png" alt="" class="wp-image-148" srcset="/wp-content/uploads/2018/07/image-2.png 511w, /wp-content/uploads/2018/07/image-2-285x300.png 285w" sizes="(max-width: 511px) 100vw, 511px" /></figure>

  * Select your newly created executable and change the .msi name if desired<figure class="wp-block-image aligncenter">

<img src="/wp-content/uploads/2018/07/image-3.png" alt="" class="wp-image-149" srcset="/wp-content/uploads/2018/07/image-3.png 511w, /wp-content/uploads/2018/07/image-3-285x300.png 285w" sizes="(max-width: 511px) 100vw, 511px" /></figure>

  * In the installer options, select &#8220;Per Machine&#8221; under &#8220;MSI installation context&#8221;. This is required to allow you to deploy to devices as opposed to users. If users are your targets, set this to &#8220;Per User&#8221;<figure class="wp-block-image aligncenter">

<img src="/wp-content/uploads/2018/07/image-4.png" alt="" class="wp-image-150" srcset="/wp-content/uploads/2018/07/image-4.png 511w, /wp-content/uploads/2018/07/image-4-285x300.png 285w" sizes="(max-width: 511px) 100vw, 511px" /></figure>

  * Give your application and ID and generate an Upgrade Code. This will allow you to uninstall and supersede if needs be, so keep a note of it and use the same code for any new iterations. Intune will notify you if the codes do not match<figure class="wp-block-image aligncenter">

<img src="/wp-content/uploads/2018/07/image-5.png" alt="" class="wp-image-151" srcset="/wp-content/uploads/2018/07/image-5.png 511w, /wp-content/uploads/2018/07/image-5-285x300.png 285w" sizes="(max-width: 511px) 100vw, 511px" /></figure>

  * Enter some information about the product including the name, the manufacturer, and the version number<figure class="wp-block-image aligncenter">

<img src="/wp-content/uploads/2018/07/image-6.png" alt="" class="wp-image-152" srcset="/wp-content/uploads/2018/07/image-6.png 511w, /wp-content/uploads/2018/07/image-6-285x300.png 285w" sizes="(max-width: 511px) 100vw, 511px" /></figure>

  * Optionally, set up contact links<figure class="wp-block-image aligncenter">

<img src="/wp-content/uploads/2018/07/image-7.png" alt="" class="wp-image-153" srcset="/wp-content/uploads/2018/07/image-7.png 511w, /wp-content/uploads/2018/07/image-7-285x300.png 285w" sizes="(max-width: 511px) 100vw, 511px" /></figure>

  * For install and uninstall arguments, use the flag &#8220;/S&#8221; (make sure this is capital). This is the standard NSIS silent install flag<figure class="wp-block-image aligncenter">

<img src="/wp-content/uploads/2018/07/image-8.png" alt="" class="wp-image-154" srcset="/wp-content/uploads/2018/07/image-8.png 511w, /wp-content/uploads/2018/07/image-8-285x300.png 285w" sizes="(max-width: 511px) 100vw, 511px" /></figure>

  * There is no need to enter any before/after command lines
  * Hit &#8220;Build&#8221; at the end and it will generate a .msi

After this, upload your .msi to Intune, deploy it to a test group, and watch the magic happen. Please note that you cannot deploy the ADMX template and the master_preferences file at the same time as the two conflict with one another.
