---
layout: post
title:  "原盘目录结构补全脚本"
date:   2019-12-03 16:21:00 +0800
categories: bluray
tag: bluray
---

* content
{:toc}

## 背景

1. 补全蓝光目录 
2. 必须是用gkb编码

---

## 代码

```vbs
Dim SourceFolder, FSO, MSG

SourceFolder = BrowseForFolder("请选择需要检查的蓝光光盘目录", 0, 17)

If SourceFolder <> "" Then

	Dim Folder(15)
	Folder(0) = SourceFolder
	Folder(1) = "BDMV"
	Folder(2) = "BDMVAUXDATA"
	Folder(3) = "BDMVBACKUP"
	Folder(4) = "BDMVBACKUPBDJO"
	Folder(5) = "BDMVBACKUPCLIPINF"
	Folder(6) = "BDMVBACKUPJAR"
	Folder(7) = "BDMVBACKUPPLAYLIST"
	Folder(8) = "BDMVBDJO"
	Folder(9) = "BDMVCLIPINF"
	Folder(10) = "BDMVJAR"
	Folder(11) = "BDMVMETA"
	Folder(12) = "BDMVPLAYLIST"
	Folder(13) = "BDMVSTREAM"
	Folder(14) = "CERTIFICATE"
	Folder(15) = "CERTIFICATEBACKUP"
 
	Set FSO = Wscript.CreateObject("Scripting.FileSystemObject")
 
	For i = 1 To UBound(Folder)
		If Not FSO.FolderExists(SourceFolder & Folder(i)) Then
			FSO.CreateFolder(SourceFolder & Folder(i))
			MSG = MSG & Folder(i) & vbCrLf
		End If
	Next
 
	If MSG = "" Then
		MSG = SourceFolder & "目录下的蓝光光盘目录结构完整"
	Else
		MSG = "以下目录缺失并已修复" & vbCrLf & vbCrLf & MSG
	End If
 
Else
 
	MSG = "所指定的路径无效"
 
End If
 
MsgBox MSG , 64, "完成"
 
Function BrowseForFolder(ByVal pstrPrompt, ByVal pintBrowseType, ByVal pintLocation)
	Dim ShellObject, pstrTempFolder
	Set ShellObject = WScript.CreateObject("Shell.Application")
	On Error Resume Next
	Set pstrTempFolder = ShellObject.BrowseForFolder(0, pstrPrompt, pintBrowseType, pintLocation)
	BrowseForFolder = pstrTempFolder.ParentFolder.ParseName(pstrTempFolder.Title).Path
	If Err.Number <> 0 Then BrowseForFolder = ""
	Set pstrTempFolder = Nothing
	Set ShellObject = Nothing
End Function
```

---
持续更新

---

[jekyll]:      http://jekyllrb.com
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-help]: https://github.com/jekyll/jekyll-help
