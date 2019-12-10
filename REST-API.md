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
+ Syntax: **GET** ` https://ruckzuck.tools/rest/v2/geturl[?customerid={customerkey}]`  
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
+ Syntax: **GET** ` https://{URL}/geticon[?shortname={product shortname}][?iconhash={MD5 hash of the icon}][&size={size in px}]`  
+ Response: Bitmap (binary)
## PowerShell Example:
```powershell 
Invoke-RestMethod -Uri "https://cdn.ruckzuck.tools/rest/v2/geticon?size=32&shortname=ruckzuck"
```
as URL
[`https://cdn.ruckzuck.tools/rest/v2/geticon?shortname=ruckzuck`](https://cdn.ruckzuck.tools/rest/v2/geticon?shortname=ruckzuck)

# API entities
C# classes with a few comments that represent entities returned by API. Note: API always returns collection of these entities.

```C#
    /// <summary>
    /// Entities returned by getcatalog API request, contain basic info about software package in RuckZuck repo
    /// </summary>
    public class CatalogItem
    {
        public DateTime Timestamp { get; set; }
        public string Description { get; set; }
        public int Downloads { get; set; }
        public string IconHash { get; set; }
        public string Manufacturer { get; set; }
        /// <summary>
        /// Date when current version of package was added to RuckZuck repo
        /// </summary>
        public DateTime? ModifyDate { get; set; }
        public string ProductName { get; set; }
        public string ProductURL { get; set; }
        public string ProductVersion { get; set; }
        public string ShortName { get; set; }
        public string[] Categories { get; set; }
        /// <summary>
        /// SoftwareId uniquely identifies a package. If package version is updated it will be assigned a new SWId.
        /// </summary>
        public int SWId { get; set; }
    }

    /// <summary>
    /// Entities returned by getsoftwares API request, contain all info about software package in RuckZuck repo
    /// </summary>
    public class SoftwareInfo
    {
        public string ProductName { get; set; }
        public string Manufacturer { get; set; }
        public string Description { get; set; }
        public string Shortname { get; set; }
        public string ProductURL { get; set; }
        public string ProductVersion { get; set; }
        public string MSIProductID { get; set; }
        public string Architecture { get; set; }
        public string PSUninstall { get; set; }
        /// <summary>
        /// Script that determines if package is already installed, usually by checking registry
        /// </summary>
        public string PSDetection { get; set; }
        /// <summary>
        /// Install script, part 2 of 3, all packages have it.
        /// </summary>
        public string PSInstall { get; set; }
        /// <summary>
        /// A prerequisite for running PSDetection script. If it returns $true PSDetection can be run.
        /// </summary>
        public string PSPreReq { get; set; }
        /// <summary>
        /// Install script, part 1 of 3, only some packages have it
        /// </summary>
        public string PSPreInstall { get; set; }
        /// <summary>
        /// Install script, part 3 of 3, only some packages have it
        /// </summary>
        public string PSPostInstall { get; set; }
        public string ContentID { get; set; }
        public List<File> Files { get; set; }
        public string Category { get; set; }
        /// <summary>
        /// Contains package dependencies (mostly VCRedist/.NET/Java, sometimes other apps).
        /// </summary>
        public string[] PreRequisites { get; set; }
        /// <summary>
        /// SoftwareId uniquely identifies a package. If package version is updated it will be assigned a new SWId.
        /// </summary>
        public int SWId { get; set; }
        public string IconHash { get; set; }
        public string IconURL { get; set; }
    }

    public class File
    {
        public string URL { get; set; }
        public string FileName { get; set; }
        public string FileHash { get; set; }
        /// <summary>
        /// Can be one of the following:
        /// "X509" - FileHash contains certificate hash of signed file
        /// "MD5" - FileHash contains MD5
        /// </summary>
        public string HashType { get; set; }
        public long? FileSize { get; set; }
    }