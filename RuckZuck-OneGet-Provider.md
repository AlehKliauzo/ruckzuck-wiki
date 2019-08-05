# Installation  
Install the MSI (x64 only) from:
https://github.com/rzander/ruckzuck/releases/download/1.7.0.3/RuckZuck.provider.for.OneGet_x64.msi

or

Install it from [RuckZuck.exe](https://github.com/rzander/ruckzuck/releases/download/1.7.0.2/RuckZuck.exe) ...

# Update
Find all applicable Updates:  
```Find-Package -ProviderName RuckZuck -Updates```

Find and Install all applicable Updates:  
```Find-Package -ProviderName RuckZuck -Updates | Install-Package```

# Search Software
Search for a Software that contains a value:  
```Find-Package -ProviderName RuckZuck -Contains "Zip"```

Find a Software based on the Shortname:  
```Find-Package -ProviderName RuckZuck "7-Zip"```

Find a Software based on the ProductName:  
```Find-Package -ProviderName RuckZuck "7-Zip 18.06 (x64)"```

# Check installed Software
Check all installed SoftwareItems that are registered in RuckZuck  
```Get-Package -ProviderName RuckZuck```

Get the ShortName of all installed SoftwareItems that are registered in RuckZuck  
```Get-Package -ProviderName RuckZuck | select PackageFilename```

Get a specific product and show all attributes:  
```Get-Package -ProviderName RuckZuck "7-Zip 18.06 (x64)" | fl``` 

# Install Software
Install a Software based on the Shortname  
```Install-Package -ProviderName RuckZuck "7-Zip"```

Install a Software based on the ProductName and skip the Installation of dependencies  
```Install-Package -ProviderName RuckZuck "7-Zip 16.04 (x64)" -SkipDependencies```

Install a Software based on the Shortname and download the Content to C:\temp  
```Install-Package -ProviderName RuckZuck "7-Zip" -LocalPath "C:\Temp"```

Install an older Version of a Software by using an alternative ProductName and ProductVersion:
```Install-Package -Provider RuckZuck -Name winrar -ProductVersion "5.70.0" -ProductName "WinRAR 5.70 (64-bit)"```

# Uninstall Software
Uninstall Software based on the ProductName  
```Uninstall-Package -ProviderName RuckZuck "7-Zip 18.06 (x64)"```


