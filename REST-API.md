# Get Gatalog:
```powershell 
$cat = Invoke-RestMethod -Uri "https://cdn.ruckzuck.tools/rest/v2/getcatalog"
```

## list ShortNames and download count
```powershell 
$cat | select ShortName, Downloads | Sort-Object ShortName
```