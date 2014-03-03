---
layout: post
title: "Using Restricted Groups"
categories: 域
tags: 
 - 域
--- 

# Using Restricted Groups

Using Restricted Groups

If you are a medium or large sized organization, you might have thousands of clients and hundreds of servers that you need to manage. Manually trying to manage all of the local groups on all of these computers is difficult, and almost impossible. Have no fear, Group Policy Objects (GPOs) are here! GPOs provide a mechanism that allows you to control the membership in local groups, and even domain groups, on any computer in the Active Directory enterprise. The specific configuration that you use for this task is the Restricted Groups GPO setting.

 

Do you have users of Windows 2000 and XP Professional computers removing the Domain Administrators group from the local Administrators group on their computer? Do you need to have a special user account placed in the Administrator group of every computer on the network for remote administrative functions?

These are common problems that most network administrators face in a Windows environment. The main problem is that it is hard to control the membership in the local groups on clients and servers throughout the enterprise. If you are a medium or large sized organization, you might have thousands of clients and hundreds of servers that you need to manage. Manually trying to manage all of the local groups on all of these computers is difficult, and almost impossible.

Have no fear, Group Policy Objects (GPOs) are here! GPOs provide a mechanism that allows you to control the membership in local groups, and even domain groups, on any computer in the Active Directory enterprise. The specific configuration that you use for this task is the Restricted Groups GPO setting.

**Where do you find Restricted Groups?**

Restricted Groups are a node within all GPOs. In this instance, I am only referring to GPOs that reside within Active Directory, not for the local GPO that exists on each computer. The Restricted Groups node exists under the Computer Configuration|Windows Settings|Security Settings node for any GPO in Active Directory. You can see this path and the Restricted Groups node in Figure 1.

![http://www.windowsecurity.com/img/upl/image0021101382950147.jpg]()
**Figure 1:** Restricted Groups node within a common GPO in Active Directory

Two things of value to notice about the graphic. First, the Restricted Groups policy affects the computer account, not the user accounts. Therefore, you will need to target the GPOs where you configure Restricted Groups to organizational units (OUs) that contain computer accounts.

The other point that I want to make about Restricted Groups is that they are not configured by default. No new GPO has Restricted Groups configured initially. The two default GPOs, Default Domain Policy and Default Domain Controller Policy, don’t have any Restricted Groups configured by default either.

**What can a Restricted Group provide?**

The Restricted Group setting allows you to configure membership in groups within Active Directory or in the local security accounts manager (SAM) of clients and servers that have joined the domain. Since the Restricted Group setting is only available in a GPO linked to an Active Directory node, the setting is centralized for both administration and deployment.

To create a Restricted Group, you only need to create a GPO, then access the Restricted Groups node as described above. Once at the Restricted Groups node, you will right-click on it and select Add Group. Enter the Group name, or browse for it in the Active Directory database. After you create the group, it will show up in the right hand pane under the Group Name column.

There are two different ways to control the membership of groups using Restricted Groups. The first controls the membership of a specified group, while the other setting control which groups the specified group has membership within.

* **Members of this group** – This setting allows you to control the members of the group that you specify for the policy. The members can include both user and group accounts. When you configure the members of a group, it will overwrite the existing membership of the group and replace the members with those specified within the GPO. If you were to configure this setting and leave the members blank, then the group would not have any members after the GPO applied to the computer.
To configure the members of a Restricted Group, you will double-click the group name that you created under the Restricted Group node. This will open up the group Properties sheet. Then, you will click the Add button for the “Members of this group” section of the form, as shown in Figure 2.

![http://www.windowsecurity.com/img/upl/image0041101382950162.jpg]()
**Figure 2:** Configuration interface to add members to a Restricted Group.

* **This group is a member of** – This setting allows you to control which other groups the specified group has membership in. All groups that you configure in this interface must meet the approved group nesting rules. Therefore, you can’t configure a local group to have membership in another group, since local groups can’t be placed in Active Directory groups, nor placed in other local groups. If the list of groups in this section is left blank, it will not remove the specified group from any existing groups, it will just not place it in additional groups.  
To configure the membership in other groups of a Restricted Group, you will double-click the group name that you created under Restricted Group node. This will open up the group Properties sheet. Then, you will click the Add button for the “This group is a member of” section of the form, as shown in Figure 3.

![http://www.windowsecurity.com/img/upl/image0061101382950162.jpg]()
**Figure 3:** Configuration interface to add groups to be a member of other groups within the Restricted Group policy.

**Common uses for Restricted Groups**

There are some common uses for Restricted Groups in companies of all sizes. Here are some ways that you can take full advantage of Restricted Groups for different situations on clients, servers, and domain controllers.

* Control the membership of the local Administrators group on all client computers to include the following accounts:

* Administrator (local SAM account)
* Domain Admins
* SMS or other remote admin domain account

Another indirect benefit of using the Restricted Group setting is that it will automatically remove any local user accounts that should not be added to the Administrators group. This typically includes local user accounts that have been created by the user of the computer, to bypass domain security.

* Control the membership of the Enterprise Admins and Schema Admins groups. These groups should not be used that often, unless a large and important change is going to occur to a portion of Active Directory. By using the Restricted Group setting, you can manage and control the membership better and ensure that an incorrect account is not added to these groups incorrectly.
* Control the membership of local groups on file servers. File servers typically have many local groups. These local groups typically nest global groups from Active Directory. You can use the Restricted Groups to keep the membership of these groups consistent and persistent from a central location of Active Directory.

**Tips for Restricted Groups**

Here are some general tips to ensure that your implementation of Restricted Groups goes smoothly.

* Make sure that the GPOs are linked to OUs that contain computer accounts
* Make sure you don’t try to configure invalid group nesting rules
* When adding members to a group, you will need to add all members through the GPO
* Use the Restricted Groups to control membership of key administrative groups within Active Directory
* Test the implementation thoroughly before you deploy, to ensure you don’t prohibit a users capabilities or lock yourself out of a computer

 
