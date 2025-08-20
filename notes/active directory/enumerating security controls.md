htb academy [link](https://academy.hackthebox.com/module/143/section/1459) 

getting windows defender status:
```powershell
Get-MpComputerStatus
```

getting app locker info, which softwares are blocked or approved :
```powershell
Get-AppLockerPolicy -Effective | select -ExpandProperty RuleCollections
```

 PowerShell Constrained Language Mode:
 ```powershell
$ExecutionContext.SessionState.LanguageMode
```

now laps is interesting
it the thing which can lead to juicy fruits like passwords..the user with extended rights can read has ability to read passwords.

github repo: [LAPSToolkit](https://github.com/leoloobeek/LAPSToolkit)

powershell:

finding users with extended rights  using Find-LAPSDelegatedGroups
```powershell
Find-LAPSDelegatedGroups
```

```powershell
Find-AdmPwdExtendedRights
```

Using Get-LAPSComputers:
```powershell
Get-LAPSComputers
```
