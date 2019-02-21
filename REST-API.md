# Get Gatalog:
```powershell 
$cat = Invoke-RestMethod -Uri "https://cdn.ruckzuck.tools/rest/v2/getcatalog"
```

## list ShortNames and download count
```powershell 
$cat | select ShortName, Downloads | Sort-Object ShortName
```

# Get Software
## from ShortName
```powershell 
Invoke-RestMethod -Uri "https://cdn.ruckzuck.tools/rest/v2/getsoftwares?shortname=sccmclictr"
```

## from ProductName, Version and Manufacturer
```powershell 
Invoke-RestMethod -Uri "https://cdn.ruckzuck.tools/rest/v2/getsoftwares?name=ruckzuck&ver=1.6.2.14&man=Zander%20Tools"
```

# Get Icon 

## as URL
[`https://cdn.ruckzuck.tools/rest/v2/geticon?shortname=ruckzuck`](https://cdn.ruckzuck.tools/rest/v2/geticon?shortname=ruckzuck)

## from ShortName
```powershell 
Invoke-RestMethod -Uri "https://cdn.ruckzuck.tools/rest/v2/geticon?shortname=ruckzuck"
```