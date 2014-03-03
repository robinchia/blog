---
layout: post
title: "使用WMI修改文件文件夹的NTFS权限"
categories: 脚本
tags: 
 - 脚本
 - VB
--- 

# 使用WMI修改文件文件夹的NTFS权限

# 使用WMI修改文件文件夹的NTFS权限

使用WMI修改文件文件夹的NTFS权限

 
01
 
strUser = 

"guests"

02
 
strPath = 

"D:\\abc.txt"
03
 
RetVal = AddPermission(strUser,strPath,

"R"

,True)

04
  
05
 
'-------------------------------------------------------------------------

06
  
07
 
'用于给文件和文件夹添加一条权限设置.返回值: 0-成功,1-账户不存在,2-路径不存在

08
 
'strUser表示用户名或组名
09
 
'strPath表示文件夹路径或文件路径

10
 
'strAccess表示允许权限设置的字符串,字符串中带有相应字母表示允许相应权限: R-读,C-读写,F-完全控制
11
 
'blInherit表示是否继承父目录权限.True为继承,False为不继承

12
  
13
 
Function AddPermission(strUser,strPath,strAccess,blInherit)

14
 
        

Set objWMIService = GetObject(

"winmgmts:\\.\root\Cimv2"

)
15
 
        

Set fso = CreateObject(

"Scripting.FileSystemObject"

)

16
 
        

'得到Win32_SID并判断用户/组/内置账户是否存在
17
 
        

Set colUsers = objWMIService.ExecQuery(

"SELECT * FROM Win32_Account WHERE Name='"

&strUser&

"'"

)

18
 
        

If colUsers.count<>0 Then
19
 
                

For Each objUser In colUsers

20
 
                        

strSID = objUser.SID
21
 
                

Next

22
 
        

Else
23
 
                

AddPermission = 1

24
 
                

Exit Function
25
 
        

End If

26
 
        

Set objSID = objWMIService.Get(

"Win32_SID.SID='"

&strSID&

"'"

)
27
 
        

'判断文件/文件夹是否存在

28
 
        

pathType = 

""
29
 
        

If fso.fileExists(strPath) Then pathType = 

"FILE"

30
 
        

If fso.folderExists(strPath) Then pathType = 

"FOLDER"
31
 
        

If pathType = 

""
 

Then

32
 
                

AddPermission = 2
33
 
                

Exit Function

34
 
        

End If
35
 
        

'设置Trustee

36
 
        

Set objTrustee = objWMIService.Get(

"Win32_Trustee"

).SpawnInstance_()
37
 
        

objTrustee.Domain = objSID.ReferencedDomainName

38
 
        

objTrustee.Name = objSID.AccountName
39
 
        

objTrustee.SID = objSID.BinaryRepresentation

40
 
        

objTrustee.SidLength = objSID.SidLength
41
 
        

objTrustee.SIDString = objSID.Sid

42
 
        

'设置ACE
43
 
        

Set objNewACE = objWMIService.Get(

"Win32_ACE"

).SpawnInstance_()

44
 
        

objNewACE.Trustee = objTrustee
45
 
        

objNewACE.AceType = 0

46
 
        

If InStr(UCase(strAccess),

"R"

) > 0 Then objNewACE.AccessMask = 1179817
47
 
        

If InStr(UCase(strAccess),

"C"

) > 0 Then objNewACE.AccessMask = 1245631

48
 
        

If InStr(UCase(strAccess),

"F"

) > 0 Then objNewACE.AccessMask = 2032127
49
 
        

If pathType = 

"FILE"
 

And blInherit = True Then objNewACE.AceFlags = 16

50
 
        

If pathType = 

"FILE"
 

And blInherit = False Then objNewACE.AceFlags = 0
51
 
        

If pathType = 

"FOLDER"
 

And blInherit = True Then objNewACE.AceFlags = 19

52
 
        

If pathType = 

"FOLDER"
 

And blInherit = False Then objNewACE.AceFlags = 3
53
 
        

'设置SD

54
 
        

Set objFileSecSetting = objWMIService.Get(

"Win32_LogicalFileSecuritySetting.Path='"

&strPath&

"'"

)
55
 
        

Call objFileSecSetting.GetSecurityDescriptor(objSD)

56
 
        

blSE_DACL_AUTO_INHERITED = True
57
 
        

If (objSD.ControlFlags And &H400) = 0 Then

58
 
                

blSE_DACL_AUTO_INHERITED = False
59
 
                

objSD.ControlFlags = (objSD.ControlFlags Or &H400)                '自动继承位置位,如果是刚创建的目录或文件该位是不置位的,需要置位

60
 
        

End If
61
 
        

If blInherit = True Then

62
 
                

objSD.ControlFlags = (objSD.ControlFlags And &HEFFF)        '阻止继承复位
63
 
        

Else

64
 
                

objSD.ControlFlags = (objSD.ControlFlags Or &H1400)                '阻止继承位置位,自动继承位置位
65
 
        

End If

66
 
        

objOldDacl = objSD.Dacl
67
 
        

ReDim objNewDacl(0)

68
 
        

Set objNewDacl(0) = objNewACE
69
 
        

If IsArray(objOldDacl) Then                '权限为空时objOldDacl不是集合不可遍历

70
 
                

For Each objACE In objOldDacl
71
 
                        

If (blSE_DACL_AUTO_INHERITED=False And blInherit=True) Or ((objACE.AceFlags And 16)>0 And (blInherit=True) Or (LCase(objACE.Trustee.Name)=LCase(strUser))) Then

72
 
                                

'Do nothing
73
 
                                

'当自动继承位置位为0时即使时继承的权限也会显示为非继承,这时所有权限都不设置

74
 
                                

'当自动继承位置位为0时,在继承父目录权限的情况下不设置继承的权限.账户和需要加权限的账户一样时不设置权限
75
 
                        

Else

76
 
                                

Ubd = UBound(objNewDacl)
77
 
                                

ReDim preserve objNewDacl(Ubd+1)

78
 
                                

Set objNewDacl(Ubd+1) = objACE
79
 
                        

End If

80
 
                

Next
81
 
        

End If

82
 
        

objSD.Dacl = objNewDacl
83
 
        

'提交设置修改

84
 
        

Call objFileSecSetting.SetSecurityDescriptor(objSD)
85
 
        

AddPermission = 0

86
 
        

Set fso = Nothing
87
 
End Function

来源： <[http://my.oschina.net/veterans/blog/27840](http://my.oschina.net/veterans/blog/27840)>
 
