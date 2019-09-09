# Summary
To host a "private" repository, there are two options possible:
* Standalone RuckZuck Server
  * No Software-Update detection possible !!!
* RuckZuck Proxy Server with Repository overrides
  * You can override/customize or remove specific Apps in RuckZuck. All other requests will be forwarded to the public RuckZuck.tools Server.

# Requirements
* .NET Core 2.2
  * IIS hosting: https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/iis/?view=aspnetcore-2.2
# Installation
Download: https://github.com/rzander/ruckzuck/releases/download/1.7.0.5/RZ.Server.Proxy.zip

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
To add a Software to the Catalog, place a RuckZuck-JSON file in ```wwwroot\repository```. The Filename must match with the Shortname of the Software.
>**Note:** RuckZuck does cache the Catalog. Restart the Server to reload the Catalog from the JSON-Files. 
