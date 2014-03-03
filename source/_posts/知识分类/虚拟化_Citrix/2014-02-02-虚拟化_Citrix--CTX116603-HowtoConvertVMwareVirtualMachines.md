---
layout: post
title: "CTX116603 - How to Convert VMware Virtual Machines"
categories: 虚拟化_Citrix
tags: 
 - 虚拟化_Citrix
--- 

# CTX116603 - How to Convert VMware Virtual Machines to XenServer Virtual Machines - Citrix Knowledge Center

![Citrix Logo]()

[Knowledge Center](http://support.citrix.com/)
[Citrix](http://www.citrix.com/ "Citrix Homepage")

[Knowledge Center](http://support.citrix.com/) [Communities](http://community.citrix.com/) [Support Forums](http://forums.citrix.com/support/) [Blogs](http://community.citrix.com/blogs/)
[Alerts]()     [Sign in]()

[Knowledge Center Home](http://support.citrix.com/) > CTX116603

Rate this Article:

![You must be signed in to rate again]( "You must be signed in to rate again")
[![]( "Article Feedback")](http://support.citrix.com/article/CTX116603#) [Article Feedback](http://support.citrix.com/article/CTX116603#) [![]( "Article Feedback")](http://support.citrix.com/article/CTX116603?print) [Print View](http://support.citrix.com/article/CTX116603?print)   [![]()](http://www.addthis.com/bookmark.php)  Languages:  

*   Select

* [日本語](http://support.citrix.com/article/CTX121395)
# How to Convert VMware Virtual Machines to XenServer Virtual Machines

Document ID: **CTX116603**   /   Created On: Jun 17, 2008   /   Updated On: Aug 18, 2009

**Average Rating:** ![4]( "4") (14 ratings)
### [View products this document applies to ![link to products this document applies to]()](http://support.citrix.com/article/CTX116603#prodrelated)

[]()

**Summary**

This document describes the two methods available to convert a VMware-formatted virtual machine (VM) into a Citrix XenServer virtual machine. The first method of converting OVF packages exported directly from VMware is the preferred as it is the quickest, most efficient and allows you to convert multiple virtual drives at the same time. The second method of converting VMDK files should be used as an alternative as it only allows the conversion of one drive at a time. For best results, copy the OVF template and the VMDK file to the computer that XenConvert is installed on for conversion.

**Requirements**

• Administrator access to VMware VM to be converted

• Administrator access to XenServer and/or XenCenter

• Basic knowledge on Open Virtualization Format (OVF)

You must be comfortable using VMware, a Windows computer to run the XenConvert Utility, XenServer and XenCenter.

Please reference the [Citrix XenConvert Guide](http://support.citrix.com/servlet/KbServlet/download/20644-102-332133/XenConvertGuide.pdf) for XenConvert Supported Operating Systems and the [Overview of the Open Virtualization Format (OVF)](http://support.citrix.com/article/CTX121652) for more information on OVF packages.

[Click here to download Citrix XenConvert Application Software](https://www.citrix.com/English/SS/downloads/details.asp?dID=36239&downloadID=1855017&pID=683148)[]()

**Initial Procedures**

1. Identify the VM you want to export.

2. Log on to that VM and uninstall **VMware Tools**. Refer to the following screen shot:

![]()

**Note**: You will experience issues if these items are not properly removed and/or uninstalled from Add or Remove Programs and from the Taskbar.

![]()

3. Delete any snapshots located with the VM.

![]()

4. Delete any unnecessary data, drives, partitions and/or applications you will no longer need for that VM.

Note:

• Enable[automount](http://technet.microsoft.com/en-us/library/cc753703(WS.10).aspx) feature for Windows VMs

• If manifest feature in VMware Workstation was enabled, delete the .mf file to allow import.

**Exporting OVF packages from VMware**

The following VMware products support OVF export:

• VMware vSphere 4

• VMware VI3

• VMware Workstation 6.5.x

• VMware OVF Tool 0.9 and 1.x

• VMware Converter 3.0.3

• VMware Converter 4.x

• VMware Studio

**Note**: The example shown here was done with VMware vSphere.

![]()

1. Select File > Export > Export OVF Template.

![]()

2. Select a Directory to store the OVF Export and ensure that Optimized for: Web (OVF) is selected.

![]()

3. A dialogue will indicate that the export completed successfully.

![]()

4. Copy the entire exported contents over to your XenConvert workstation leaving the folder structure the same.

**Converting OVF Export with XenConvert**

![]()

1. From XenConvert select the Open Virtualization Format (OVF) Package option.

**Note**: OVF packages can only be converted directly to XenServer.

![]()

2. Select the OVF Package to import and indicate whether you would like to “Verify Content” and/or “Verify Author”.

![]()

3. Enter the hostname, user name (root) and password of the XenServer that you will convert the OVF package directly to.

![]()

4. Select Convert to start the conversion process.

**Converting VMDK Files**

**Note**: XenConvert has been designed to convert a single virtual disk from VMDK format at a time. Copy data from all additional drives and partitions to an external location and delete any additional drives and/or partitions.

1. Browse the physical location of the VMware files and locate the virtual machine’s .vmdk file.

![]()

2. Make a note of the path to the virtual machine’s .vmdx file.

3. Install XenConvert application on the Windows computer that will perform the conversion.

4. Launch XenConvert and select the “VMware Virtual Hard Disk (VMDK)” option.

![]()

5. You will be presented with the following three options on converting a “VMware Virtual Hard Disk (VMDK)” for XenServer.

• XenServer option converts directly to an accessible XenServer host

• XenServer Virtual Appliance option converts to an .xva format file that can be used to import

• XenServer Virtual Hard Disk (VHD) option converts VMDK file to a .vhd file

![]()

6.All three options prompt you to browse for and select the .vmdk file to convert.

![]()

**Option 1 – XenServer**

1. You must specify the destination XenServer hostname, User name, Password and Workspace to be used during the conversion.

2. Type or browse to the location where you want the converted files to be stored. For conversion efficiency, Citrix recommends specifying a location on the local computer where you are running conversion, preferably on a different partition or drive.

**Note**: The Workspace specified will need to have enough space available to convert the selected VM.

![]()

3. Enter the name of the VM as you would like it to appear in XenCenter after it has been uploaded.

![]()

4. The XenConvert utility displays the progress of the conversion and upload of VM to XenServer.

![]()

**5. Note**: Do not close dialogue box until the Status indicates “Conversion was successful!”

![]()

6. After the conversion process completes, XenServer shows the converted VM by the name specified during conversion followed by “import”.

![]()

**Option 2 –Xen Virtual Appliance**

The Xen Virtual Appliance option converts the VM into a portable format that can be easily moved, archived or uploaded to XenServer.

1. After selecting the Xen Virtual Appliance option and VMDK file to convert you are prompted to select a folder to store the converted contents.

![]()

2. After the VMDK file is converted you have an output that lists an hda folder, ova.xml, .pvp and .vhd file.

**Note**: Do not change the file structure. The ova.xml file and **hda** folder must be on the same level.

![]()

**Importing the Converted VM to XenServer**

1. Log on to XenCenter.

2. On the menu bar, go to **VM > Import**.

![]()

You have the option to browse for the ova.xml file or choose either **Exported VM** or **Exported template**.

**Note**: The same file extension (.xva) is used for both the exported VMs and exported templates.

![]()

1. Select **XenServer Virtual Appliance Version 1 (ova.xml)** from the **Files of Type** list. You are now able to browse and see the ova.xml file.

![]()

2. Select the XenServer host that you want to deploy the imported VM to.

![]()

3. Select the storage repository where the virtual disks for the newly imported VM will be stored.

**Note**: You can copy [a VM from one storage repository to another storage repository](http://support.citrix.com/article/ctx116685) after the import process has completed.

![]()

4. Add the network interfaces you want to configure for the new VM.

![]()

5. Click **Finish** to complete the import process.

![]()

Allow enough time for the import process to complete. The XenCenter **Logs** tab displays an estimate of the amount of time that the VM will take to import.

![]()

The imported VM will have the name “import” at the end of it to identify that it has been imported. You can rename the VM after the import process finishes.

![]()

**How to Import a VM through the Command Line Interface (CLI):**

1. Copy all the files needed to a mounted share accessible by your XenServer host.

2. Run the **xe vm-import** command:**#xe vm-import filename=<path and name of ova.xml file> sr-uuid=<UUID of SR to install imported VM to>**

Example command: # xe vm-import filename=/nfs or cifs share/VMWare_WinXP_Export/ova.xml sr-uuid=da31c9d2-88ea-35f6-8c48-924db6c39817

**More Information**

[Citrix XenConvert Application Software](https://www.citrix.com/English/SS/downloads/details.asp?dID=36239&downloadID=1855017&pID=683148)

[CTX121646 - Citrix XenConvert 2.0.1 Guide](http://support.citrix.com/article/ctx121646)

[CTX116685 - How to a Copy a Virtual Machine From One Storage Repository to Another](http://support.citrix.com/article/ctx116685)

[VMware OVF Tool User Guide](http://www.vmware.com/pdf/ovf_tool.pdf)[]()
[]()

### This document applies to:

* [XenServer 3.1](http://support.citrix.com/product/legacy/xensv3.1/)
* [XenServer 3.2](http://support.citrix.com/product/legacy/xensv3.2/)
* [XenServer 4.0](http://support.citrix.com/product/xens/v4.0/)
* [XenServer 4.1](http://support.citrix.com/product/xens/v4.1/)
* [XenServer 5.0](http://support.citrix.com/product/xens/v5.0/)
* [XenServer 5.0 Update 3](http://support.citrix.com/product/xens/v5.0/)
* [XenServer 5.5](http://support.citrix.com/product/xens/v5.5/)
# Did this article resolve your problem/question?

![]()  Yes
No
Need to test first
Not sure, I need help
Just browsing/General research
### **What would you have done if this article had not solved your issue?**

### **What action will you take next?**
![]()

Open a Citrix Technical Support Case
Contact my Citrix Solution Advisor
Continue searching Knowledge Center
Search non-Citrix resources
Ignore the problem/take no further action

Thanks for your feedback!   []()[Report errors with this document](http://support.citrix.com/kc/basicLogin/show?redirectURL=/article/CTX116603/%3Ffeedback=true%23feedback)

### Use this field to report errors with this document:

[]()
Thanks for your report!

Knowledge Center

[]()

[Advanced Search](http://support.citrix.com/search/advanced/)
Products
[XenApp](http://support.citrix.com/product/xa/)
* [XenApp 5.0 for Windows Server 2008](http://support.citrix.com/product/xa/v5.0_2008/)
* [XenApp 5.0 for Windows Server 2003](http://support.citrix.com/product/xa/v5.0_2003/)
* [Presentation Server 4.5 and Components](http://support.citrix.com/product/xa/v4.5/)
* [Presentation Server 4.5 SE Edition](http://support.citrix.com/product/xa/v4.5_se2003/)
* [Presentation Server 4.0 and Components](http://support.citrix.com/product/xa/v4.0/)
* [Presentation Server 4.0 for UNIX](http://support.citrix.com/product/xa/unix/)
* [XenApp for UNIX 4.0 with Feature Pack 1](http://support.citrix.com/product/xa/unix_v4.0fp1/)
[XenDesktop](http://support.citrix.com/product/xd/)
* [XenDesktop 3.0](http://support.citrix.com/product/xd/v3.0/)
* [XenDesktop 2.1](http://support.citrix.com/product/xd/v2.1/)
* [XenDesktop 2.0](http://support.citrix.com/product/xd/v2.0/)
[XenServer](http://support.citrix.com/product/xens/)
* [XenServer 5.5](http://support.citrix.com/product/xens/v5.5/)
* [XenServer 5.0](http://support.citrix.com/product/xens/v5.0/)
* [XenServer 4.1](http://support.citrix.com/product/xens/v4.1/)
* [XenServer 4.0](http://support.citrix.com/product/xens/v4.0/)
[Essentials for Hyper-V](http://support.citrix.com/product/emhv/)
* [Essentials 1.0 for Microsoft Hyper-V](http://support.citrix.com/product/emhv/esv1.0/)
[NetScaler Application Delivery](http://support.citrix.com/product/nsad/)
* [NetScaler Application Delivery Software 9.1](http://support.citrix.com/product/nsad/v9.1/)
* [NetScaler VPX 9.1](http://support.citrix.com/product/nsad/vpx9.1/)
* [NetScaler Application Delivery Software 9.0](http://support.citrix.com/product/nsad/v9.0/)
* [NetScaler Application Delivery Software 8.1](http://support.citrix.com/product/nsad/v8.1/)
* [NetScaler Application Delivery Software 8.0](http://support.citrix.com/product/nsad/v8.0/)
* [NetScaler Application Delivery Software 7.0](http://support.citrix.com/product/nsad/v7.0/)
* [NetScaler Application Delivery Software 6.1](http://support.citrix.com/product/nsad/v6.1/)
* [NetScaler Application Delivery Software 6.0](http://support.citrix.com/product/nsad/v6.0/)
[Access Gateway](http://support.citrix.com/product/ag/)
* [Access Gateway 9.1 Enterprise Edition](http://support.citrix.com/product/ag/eev9.1/)
* [Access Gateway 9.0 Enterprise Edition](http://support.citrix.com/product/ag/eev9.0/)
* [Access Gateway 8.1 Enterprise Edition](http://support.citrix.com/product/ag/eev8.1/)
* [Access Gateway 8.0 Enterprise Edition](http://support.citrix.com/product/ag/eev8.0/)
* [Access Gateway 7.0 Enterprise Edition](http://support.citrix.com/product/ag/eev7.0/)
* [Access Gateway 4.6 Standard Edition](http://support.citrix.com/product/ag/v4.6se/)
* [Access Gateway 4.5 Advanced Edition](http://support.citrix.com/product/ag/v4.5ae/)
* [Access Gateway 4.5 Standard Edition](http://support.citrix.com/product/ag/v4.5se/)
[Branch Repeater](http://support.citrix.com/product/brrepeat/)
* [Branch Repeater 5.5](http://support.citrix.com/product/brrepeat/v5.5/)
* [Branch Repeater with Windows Server 2.0](http://support.citrix.com/product/brrepeat/v2.0/)
* [Branch Repeater 5.0](http://support.citrix.com/product/brrepeat/v5.0/)
* [Branch Repeater with Windows Server 1.5](http://support.citrix.com/product/brrepeat/v1.5/)
* [Branch Repeater 4.5](http://support.citrix.com/product/brrepeat/v4.5/)
* [Branch Repeater with Windows Server 1.0](http://support.citrix.com/product/brrepeat/v1.0/)
[Receiver](http://support.citrix.com/product/rec/)
* [Receiver for Windows](http://support.citrix.com/product/rec/win/)
* [Merchandising Server](http://support.citrix.com/product/rec/mer/)
* [Receiver for iPhone](http://support.citrix.com/product/rec/iPhone/)

[>> View All Products](http://support.citrix.com/product/)
[![]()](http://twitter.com/citrixsupport)

Knowledge Resources
* [Product Documentation](http://support.citrix.com/productdocs/)
* [Microsoft Updates](http://support.citrix.com/pages/microsoft/)
* [Licensing](http://support.citrix.com/pages/licensing/)
* [Troubleshooting](http://support.citrix.com/pages/troubleshooting/)
* [What's New...](http://support.citrix.com/pages/whatsnew/)
* [Knowledge Center FAQ](http://support.citrix.com/pages/faqs/)
* [My Support Options](http://www.citrix.com/English/SS/supportSecond.asp?slID=38676&ntref=hp_nav_US)
©1999-2009 Citrix Systems, Inc. All rights reserved.

* [Contact](http://www.citrix.com/English/contact/index.asp)
* [Careers](http://www.citrix.com/site/aboutCitrix/employment/index.asp)
* [Legal Notice](http://www.citrix.com/English/aboutCitrix/legal/legalNotice.asp)
* [Privacy](http://www.citrix.com/English/aboutCitrix/legal/privacyStatement.asp)
* [Governance](http://www.citrix.com/English/aboutCitrix/governance/index.asp)
* [Site Feedback]()
* [Site Map](http://support.citrix.com/sitemap)
