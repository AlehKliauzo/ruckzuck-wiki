# Installation  
Install the MSI (x64 only) from:
https://github.com/rzander/ruckzuck/releases/download/1.6.2.13/RuckZuck.provider.for.OneGet_x64.msi  
> Use the Parameter `WEBSERVICE=<URL>` to specify an internal Repository or [RZ.Cache](https://rzander.azurewebsites.net/ruckzuck-cache/) Server. 

or

Install it from [RuckZuck.exe](https://github.com/rzander/ruckzuck/releases/download/1.6.2.13/RuckZuck.exe) ...

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

Get a specific product and show all attributes:  
```Get-Package -ProviderName RuckZuck "7-Zip 18.06 (x64)" | fl``` 

# Install Software
Install a Software based on the Shortname  
```Install-Package -ProviderName RuckZuck "7-Zip"```

Install a Software based on the ProductName and skip the Installation of dependencies  
```Install-Package -ProviderName RuckZuck "7-Zip 16.04 (x64)" -SkipDependencies```

Install a Software based on the Shortname and download the Content to C:\temp  
```Install-Package -ProviderName RuckZuck "7-Zip" -LocalPath "C:\Temp"```

# Uninstall Software
Uninstall Software based on the ProductName  
```Uninstall-Package -ProviderName RuckZuck "7-Zip 18.06 (x64)"```


