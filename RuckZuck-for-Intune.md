# RuckZuck for Intune
RuckZuck for Intune is a standalone tool to create Intune Win32Lob Applications from the RuckZuck Repository.

What it does:
* Connect Intune by using Graph API to check for outdated RuckZuck-Applications
* User can select Applictions to create...
* download the required Files to %TEMP%\{ContentID}
* (Install Nuget Module if missing)
* (Install AzureAD PowerShell Module if missing)
* (download IntuneWinAppUtil.exe from https://github.com/Microsoft/Microsoft-Win32-Content-Prep-Tool to %TEMP%\intunewin)
* extract PowerShell script to upload App (Script is based on https://github.com/microsoftgraph/powershell-intune-samples/tree/master/LOB_Application) to %TEMP%\intunewin
* generate install.ps1, uninstall.ps1, detection.ps1, requirements.ps1 and logo.png in %TEMP%\{ContentID}
* run IntuneWinAppUtil.exe to generate *.intunewin File in %TEMP%\intunewin
* run PowerShell to create and upload Application in Intune.

> This is a **preview** release, please provide feedback if you have some issues...

# Things to know
* Run this tool on a clean client (no Software). Otherwise it can happen that the tool will generate an Application to update existing Versions (e.g. for AdobeReader)
* The "notes" field in Intune is required to store required information to detect updates. Do not clean or modify this data.
* PowerShell is used for PreReq check, Detection, Install and uninstall. The Applications will not work if PowerShell is restricted.
* PowerShell 5.1 is a PreRequisite on all Clients

# Known Issues
* You have to "Run as Admin" for the first use (to install nuget and AzureAD module)
* AzureAD Token from the first Login is not forwarded to the PowerShell-Script, so you may have to logon again

# Download
Get a preview Version:
https://github.com/rzander/ruckzuck/releases/download/1.7.1.2/RZ4Intune.exe
