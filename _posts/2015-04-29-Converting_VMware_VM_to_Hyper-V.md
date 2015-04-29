---
layout: post
title: Converting a VMware virtual machine to Hyper-V
---

I use VMware Workstation at work. I use Windows 7, I paid for Workstation a few years ago and it is pretty good. At home, I run Windows 8.1 and use Hyper-V (because I can, and it is already there). I wanted to copy my main development VM from work to use at home and I thought this would be easy. Hah!

Well, it relatively straight forward, but I spent a lot of time bashing my head against the desk, perplexed as to why Hyper-V just doesn't import the damn thing and convert it!

These steps allow you to convert the VM without requiring ESX or vCenter.

### 1. Download the virtual hard drive conversion tool ###

First thing you need to do is get the [Microsoft Virtual Machine Converter 3.0](https://www.microsoft.com/en-au/download/details.aspx?id=42497) (MVMC) from the Microsoft Download Center. This has an MSI that installs a pretty GUI program and some PowerShell cmdlets. 

Don't try the GUI. This only works with ESX or vCenter.

### 2. Convert the VMDK files to VHDX ###

Open a PowerShell command prompt.

Register the PowerShell module that was installed with MVMC. You can do that with the command:

`Import-Module "C:\Program Files\Microsoft Virtual Machine Converter\MvmcCmdlet.psd1"`

Verify the import by listing the commands:

`Get-Command -Module mvmccmdlet`

You should see something like this:

![Register PowerShell Module]({{ site.baseurl }}/images/register_ps_module.png)

Change the current directory to the location of the VMDK file(s) to convert and enter the following command to create a new VHDX file (converted from the VMDK) in the same folder.

`ConvertTo-MvmcVirtualHardDisk -SourceLiteralPath ".\Win 8.1 VM - VS2015.vmdk" -DestinationLiteralPath . -VhdType FixedHardDisk -VhdFormat Vhdx`

This conversion may take a while; it took between 30 and 40 minutes (I think - I walked away) for a 60GB disk on a Samsung 940 EVO SSD. You get a progress bar across the top of your command window if you really want to sit and wait!

![Progress]({{ site.baseurl }}/images/vmdk_conversion.png)

### 3. Create a new Hyper-V Virtual Machine ###

Move the newly created VHDX to its new home. On my machine this is set to:

`C:\Users\Public\Documents\Hyper-V\Virtual hard disks`

Open Hyper-V Manager and create a New Virtual Machine. Follow the wizard through, but when you get to the **Connect Virtual hard Disk** section, select *Use an existing virtual hard disk* and browse to the location of the VHDX file that you created.

![Progress]({{ site.baseurl }}/images/connect_vhd.png)

Continue and complete the wizard - you're done!


 
