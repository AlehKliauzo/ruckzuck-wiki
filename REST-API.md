# Get Catalog:
+ Description: get all Software-Products in the RuckZuck repository  
+ Syntax: **GET** ` https://{URL}/getcatalog[?customerid={customerkey}]`  
+ Response: JSON Array of Catalog-Items

## PowerShell Example:
```powershell 
$cat = Invoke-RestMethod -Uri "https://cdn.ruckzuck.tools/rest/v2/getcatalog"
$cat | select ShortName, Downloads | Sort-Object ShortName
```


## Get Software
### from ShortName
```powershell 
Invoke-RestMethod -Uri "https://cdn.ruckzuck.tools/rest/v2/getsoftwares?shortname=sccmclictr"
```

### from ProductName, Version and Manufacturer
```powershell 
Invoke-RestMethod -Uri "https://cdn.ruckzuck.tools/rest/v2/getsoftwares?name=ruckzuck&ver=1.6.2.14&man=Zander%20Tools"
```

## Get Icon 

### from ShortName
```powershell 
Invoke-RestMethod -Uri "https://cdn.ruckzuck.tools/rest/v2/geticon?shortname=ruckzuck"
```

#### as URL
[`https://cdn.ruckzuck.tools/rest/v2/geticon?shortname=ruckzuck`](https://cdn.ruckzuck.tools/rest/v2/geticon?shortname=ruckzuck)