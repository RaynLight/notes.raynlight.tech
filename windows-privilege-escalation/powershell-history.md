# Powershell History

Read windows powershell history using this powershell command

```powershell
gc (Get-PSReadLineOption).HistorySavePath
```

The equivalent can be done with cmd&#x20;

```powershell
type %APPDATA%\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
```
