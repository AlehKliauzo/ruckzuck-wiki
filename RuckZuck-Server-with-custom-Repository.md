# Summary
To host a "private" repository, there are two options possible:
* Standalone RuckZuck Server
  * >Note: No Software-Update detection possible !!!
* RuckZuck Proxy Server with Repository-overrides
  * You can override/customize or remove specific Apps in RuckZuck. All other requests will be forwarded to the public RuckZuck.tools Server.

# Requirements
* .NET Core 2.2
  * IIS hosting: https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/iis/?view=aspnetcore-2.2
# Installation
Download and extract the latest RZ.Server binaries: https://github.com/rzander/ruckzuck/releases/download/1.7.0.5/RZ.Server.Proxy.zip

# Run
If you start RZ.Server.exe (or call ```dotnet .\RZ.Server.dll```), you should see a screen like:
![console.png](https://user-images.githubusercontent.com/11909453/64550526-669f0080-d333-11e9-8d5b-0519db1bec18.png)

You can now open a browser and open "http://localhost:5000" to get the same Web-Frontend as https://ruckzuck.tools  

# Configuration
## Plugins
Access to the Back-end/Repository is handled with Plugins located in ```wwwroot\plugins```. The default RZ.Server does have the following plugins:
- RZ.Plugin.Catalog.dll
- RZ.Plugin.Catalog.Proxy.dll
- RZ.Plugin.Feedback.Proxy.dll
- RZ.Plugin.Software.dll
- RZ.Plugin.Software.Proxy.dll
- RZ.Plugin.SWLookup.Proxy.dll

Plugins will internally handled in an alphabetic order e.g. all "Catalog" requests will first sent to RZ.Plugin.Catalog.dll and as 2nd attempt to RZ.Plugin.Catalog.Proxy.dll. If you want a different order, you have to rename the plugins.
All Plugins ending with "Proxy.dll" will act as Proxy and forward the requests to the public RuckZuck.tools API.

To host a complete standalone Server, remove all *.proxy.dll's from the plugins directory. 
>**Note:** a Standalone Server comes with an empty repository and is unable to detect any software updates (RZ.Plugin.SWLookup.Proxy.dll is required to detect Updates)

## Catalog extensions
To add a Software to the Catalog, place a RuckZuck-JSON file in ```wwwroot\repository```. The Filename must match with the Shortname of the Software. The Server contains an example for ```DevCDRAgent```.
>**Note:** RZ.Server does cache the entire Catalog. Restart the Server to reload the Catalog from the JSON-Files. Also delete the local Catalog-Cache ```%TEMP%\rzcat.json``` on your client(s) or wait 30min. 

## Catalog removals
To remove a Software from the Catalog, place a JSON file in ```wwwroot\repository``` that just contains the **ShortName** Property.
Example ```ruckzuck provider for oneget.json```:
```JSON
[
    {
        "ShortName": "RuckZuck provider for OneGet"
    }
]
```
... this will remove ```RuckZuck provider for OneGet``` from the Catalog.

## Software definitions
**RZ.Plugin.Software.dll** will check for Software definitions in: ```wwwroot\repository\{manufacturer}\{productname}\{productversion}\{shortname}.json```

To e.g. host a customized version of ```Visual Studio Code```, place your RuckZuck-Json file in ```wwwroot\repository\microsoft corporation\microsoft visual studio code\1.38.0\code.json``` 
and restart the RZ.Server. 

Depending on the order of the Plugin DLL's, it will first lookup in the local store and if nothing is found in the public RuckZuck.tools Repository. This allows to override public packages with custom parameters or additional files/commands.

## Content
If RZ.Server detects the binaries of a Software in ```wwwroot\content\{contentid}```, it will redirect the download-URL to the local RZ.Server. Otherwise the download URL from the Software definition will be used. 
If **RZ.Plugin.Software.Proxy.dll** is in use, it will download the binaries on the first request to ```wwwroot\content\{contentid}``` so that clients will use RZ.Server as a caching proxy.