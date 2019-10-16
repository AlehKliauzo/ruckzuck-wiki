# Get URL:
+ Description: get the current REST-API URL. CustomerID can be used in the future to redirect to private or custom repositories.
+ Syntax: **GET** ` https://ruckzuck.tools/geturl[?customerid={customerkey}]`  
+ Response: string with current URL of the REST API

## PowerShell Example:
```powershell 
Invoke-RestMethod -Uri "https://ruckzuck.tools/rest/v2/geturl"
```
> Note: Do **NOT** hardcode the URL in your project, always use GetURL to get the current URL as this URL may change.

> **Important:** Future Versions of RuckZuck will require to call GetURL before you get any response from other methods! 

# Get Catalog:
+ Description: get all Software-Products in the RuckZuck repository  
+ Syntax: **GET** ` https://{URL}/getcatalog[?customerid={customerkey}]`  
+ Response: JSON Array of Catalog-Items

## PowerShell Example:
```powershell 
$url = Invoke-RestMethod -Uri "https://ruckzuck.tools/rest/v2/geturl"
$cat = Invoke-RestMethod -Uri "$($url)/rest/v2/getcatalog"
$cat | select ShortName, Downloads | Sort-Object ShortName
```

# Get Software
+ Description: get software package definitions.
+ Syntax: **GET** ` https://{URL}/getsoftwares?[shortname={product shortname}][name={productname}&ver={productversion}&man={manufacturer}][&image={true|false]`. Wildcards within the parameters are **not** supported. Use the *image=true* option to get the product icon in binary format.
+ Response: JSON Array of Software-Items. Most products have one Item, but a software can have multiple items e.g. based on architecture or language. 

example result:
```JSON
[
    {
        "ProductName": "7-Zip 19.00 (x64)",
        "Manufacturer": "Igor Pavlov",
        "Description": "7-Zip is a file archiver with a high compression ratio for ZIP and GZIP formats, which is between 2 to 10% better than its peers, depending on the exact data tested. And 7-Zip boosts its very own 7z archive format that also offers a significantly higher compression ratio than its peers. up to 40% higher!",
        "ProductURL": "http://www.7-zip.org/",
        "ProductVersion": "19.00",
        "MSIProductID": "",
        "Architecture": "X64",
        "PSUninstall": "$proc  = (Start-Process -FilePath \"$($Env:ProgramFiles)\\7-Zip\\Uninstall.exe\" -ArgumentList \"/S\" -Wait -PassThru);$proc.WaitForExit();$ExitCode = $proc.ExitCode",
        "PSDetection": "if(([version](Get-ItemPropertyValue -path 'HKLM:\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Uninstall\\7-Zip' -Name DisplayVersion -ea SilentlyContinue)) -ge '19.00') { $true } else { $false }",
        "PSInstall": "$proc = (Start-Process -FilePath \"7z1900-x64.exe\" -ArgumentList \"/S\" -Wait -PassThru);$proc.WaitForExit();$ExitCode = $proc.ExitCode",
        "PSPreReq": "[Environment]::Is64BitProcess",
        "PSPreInstall": "try { $proc = (Start-Process -FilePath \"msiexec.exe\" -ArgumentList \"/x {23170F69-40C1-2702-1805-000001000000} /qn REBOOT=REALLYSUPPRESS\" -Wait -PassThru);$proc.WaitForExit(); $proc = (Start-Process -FilePath \"msiexec.exe\" -ArgumentList \"/x {23170F69-40C1-2702-1900-000001000000} /qn REBOOT=REALLYSUPPRESS\" -Wait -PassThru);$proc.WaitForExit() } catch{}\r\n",
        "PSPostInstall": "",
        "ContentID": "b3050264-2188-43b0-8412-a4663af1b437",
        "Files": [
            {
                "URL": "https://7-zip.org/a/7z1900-x64.exe",
                "FileName": "7z1900-x64.exe",
                "FileHash": "D7B20F933BE6CDAE41EFBE75548EBA5F",
                "HashType": "MD5"
            }
        ],
        "Category": "Compression",
        "PreRequisites": [],
        "SWId": 19053768,
        "IconHash": "9qZUxGc2ukJWbm83ztdYZ135f",
        "IconURL": "https://cdn.ruckzuck.tools/rest/v2/geticon?iconhash=9qZUxGc2ukJWbm83ztdYZ135f",
        "ShortName": "7-Zip"
    }
]
```

## PowerShell Example:
```powershell 
Invoke-RestMethod -Uri "https://cdn.ruckzuck.tools/rest/v2/getsoftwares?shortname=sccmclictr"
```
by using ProductName, Version and Manufacturer:
```powershell 
Invoke-RestMethod -Uri "https://cdn.ruckzuck.tools/rest/v2/getsoftwares?name=ruckzuck&ver=1.6.2.14&man=Zander%20Tools"
```

# Get Icon 
+ Description: get the product icon 
+ Syntax: **GET** ` https://{URL}/geticon[?shortname={product shortname}][?iconhash={MD5 hash of the icon}][&size={size in px})`  
+ Response: Bitmap (binary)
## PowerShell Example:
```powershell 
Invoke-RestMethod -Uri "https://cdn.ruckzuck.tools/rest/v2/geticon?size=32&shortname=ruckzuck"
```
as URL
[`https://cdn.ruckzuck.tools/rest/v2/geticon?shortname=ruckzuck`](https://cdn.ruckzuck.tools/rest/v2/geticon?shortname=ruckzuck)