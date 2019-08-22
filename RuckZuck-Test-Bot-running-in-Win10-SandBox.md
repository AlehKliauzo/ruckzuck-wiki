With Windows 10 (1903 or newer), you can use the Windows Sand Box to automatically test failed RuckZuck installations. This will help to identify real Problems and it will automatically clean out false positives.

You can use the following XML File and save is as ".WSB" (Windows Sand Box) File. 
```XML
<Configuration>
<VGpu>Disable</VGpu>
<Networking>Enable</Networking>
<MappedFolders>
   <MappedFolder>
   </MappedFolder>
</MappedFolders>
<LogonCommand>
   <Command>powershell.exe -noexit -command "&amp; {cd $env:temp;if(-NOT (Test-Path RZBot.exe)) { $wc = New-Object System.Net.WebClient; $wc.DownloadFile('https://github.com/rzander/ruckzuck/releases/download/1.7.0.5/RZBot.exe', """$env:temp\RZBot.exe""") };&amp;$env:temp\RZBot.exe}"</Command>
</LogonCommand>
</Configuration>
```

# Requirements
* The Tool will stop after running for more than 6 hours
* The Tool will not start if there is any Software installed, so you need a clean machine-