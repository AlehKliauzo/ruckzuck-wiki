With Windows 10 (1903 or newer), you can use the Windows Sand Box to automatically test failed RuckZuck installations. This will help to identify real Problems and it will automatically clean out false positives.

You can use the following XML File and save is as ".WSB" (Windows Sand Box) File. 
```XML
<Configuration>
<VGpu>Disable</VGpu>
<Networking>Enable</Networking>
<LogonCommand>
   <Command>powershell.exe -noexit -command "&amp; {cd $env:temp;if(-NOT (Test-Path RZBot.exe)) { $wc = New-Object System.Net.WebClient; $wc.DownloadFile('https://github.com/rzander/ruckzuck/releases/download/1.7.1.2/RZBot.exe', """$env:temp\RZBot.exe""") };&amp;$env:temp\RZBot.exe}"</Command>
</LogonCommand>
</Configuration>
```
To start the SandBox, just double-click the .WSB File...

The SandBox will first download and run RZBot.exe (this can take a moment). RZBot.exe will connect to the RuckZuck Service Bus where all failed installations will queue up. If RZBot is able to install the Software without errors, the Software will be removed from the queue. At the end, only real failures will stay in the queue.

# Requirements
* Windows 10 (>= 1903) with SandBox-Feature enabled:  `Enable-WindowsOptionalFeature -FeatureName "Containers-DisposableClientVM" -Online`

# Things to know
* Do not run the RZ.Bot in an Environment with a local RuckZuck Server. The goal is to detect broken links etc. and this will not happen if a local Server is involved.
* The Tool will not start if there is any other Software installed, so you need a clean machine!
* The Tool will stop after running for more than 6 hours.
* The Queue may contains software which is already updated.
* RZBot will not increase the "Downloads" counter on Software.