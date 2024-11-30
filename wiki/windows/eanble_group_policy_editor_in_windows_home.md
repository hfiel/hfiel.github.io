# How to enable the group policiy editor (gpedit.msc) on Windows Home

(I have personally tested this working OK on a Windows 11 Home machine running version 23H2)

Via https://www.guidingtech.com/fix-group-policy-editor-gpedit-missing-windows-10/

1. Open a command prompt as administrator
2. Run these two commands:

```
FOR %F IN ("%SystemRoot%\servicing\Packages\Microsoft-Windows-GroupPolicy-ClientTools-Package~*.mum") DO ( DISM /Online /NoRestart /Add-Package:"%F" )
```

```
FOR %F IN ("%SystemRoot%\servicing\Packages\Microsoft-Windows-GroupPolicy-ClientExtensions-Package~*.mum") DO (DISM /Online /NoRestart /Add-Package:"%F")
```
