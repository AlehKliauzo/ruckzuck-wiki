RuckZuck for Configuration Manager is a Console-Extension for System Center Configuration Manager which allows to import Applications from the [RuckZuck Repository](https://ruckzuck.tools/Repository.aspx). The Tool is "free" Software without support and warranty. The Software Repository is open for everyone in the hope that the community is helping to maintain the repository...

With a Repository of over 400 Applications, RuckZuck for Configuration Manager can be used to manage and update 3rd Party Apps in Configuration Manager with just a few clicks...

### What's new with V1.7.0.5
- If the detection Method contains HKCU: the generated DeploymentType will run as User (instead of SYSTEM)
- generated Bootstrap .exe's are using the V2 RuckZuck REST API

### What's new with V1.5.1.8
- RZ4CM is now using the same UI as the [RuckZuck standalone Tool](http://ruckzuck.tools/default.aspx). 
- The Tool is now able to detect updates for previously downloaded Apps in ConfigMgr. New Software Versions are listed in the UI so an Admin has the choice of what to update. 

### PreRequisites
- Configuration Manager (Current Branch)
- RZ4CM requires .NET 4.6
- PowerShell 5 or higher on all Clients running imported Applications
- RZ4CM requires Internet Access to the RuckZuck-Repository and to download the source Files from the Vendors Pages.

### How to update from an older Version
It's recommended to backup an older version (**RZ4CM.exe** and **RZ4CM.exe.config**) just for the case, you have to switch back. The Tools is a single executable located in the same Directory as the CM Console ( *..\Microsoft Configuration Manager\AdminConsole\bin* ). 
>Note: RZ4CM.exe.config will be reverted to default after upgrading. It's not recommended to restore the old config file as there may be new options in the new file. Please compare the new and old .config file and copy your modifications to the new file.

### Installation
You can download the Installer from [RuckZuck.tools](https://ruckzuck.tools). The Setup requires local Admin permission to extract the executable to **..\Microsoft Configuration Manager\AdminConsole\bin\RZ4CM.exe** and creates the extension in the Configuration Manager Console:
<img src="/content/images/2017/06/Extension.png" alt="Extension" style="width: 300px;"/>

### Things to know
Be Aware the RuckZuck Repository is a common repository and not specific to ConfigurationManager. So there are a few Things to keep in mind 

- RuckZuck does not grant you a license for the downloaded Products. Licensing is your Problem. Note: Some of the Tools in the Repository may be only free for private use... so check the EULAS of the Products! 
- RuckZuck will get the Content from the Vendors Web-Sites. It can happen that the URL or the content has changed and the tool is unable to download or verify the checksum of the files. Please provide feedback if you see a broken link..
- The Software in the RuckZuck repository is tested on Win10 and running interactively, it may happen that some SW will not work with ConfigMgr (running as SYSTEM). 
- The RuckZuck standalone-tool will automatically install dependencies but the App in ConfigMgr will not have any dependencies assigned.
- The RuckZuck standalone-tool can check prerequisites more granular as ConfigMgr, so you may loose these PreReq checks on the ConfigMgr Application
- RuckZuck standalone can detect and download content for different languages. RZ4CM will only download content for the current langue.
- Everyone is allowed to create an publish Software in the RuckZuck Repository until:
  - The Product can be installed silently
  - The content is downloaded from the Vendors Web-Site and the link is not password protected
  - The Product does not contain Malware, AdWare or any other illegal Code
- There are two ways to import Applications into ConfigMgr:
![](/content/images/2017/06/RZ4CM.png)
  - "Download files and create CM App.." will download the full content when creating the App. Clients can get all the files from a local DP.
  - "Create App with Bootstrap.exe" will generate a little executable (~60KB). This .exe will download the required content from the Internet when running. This allows to save disk-space in DEMO environment or to reduce Network-traffic in scenarios where the Clients are accessing the DP over Internet.

### Configuration
RZ4CM is ready to use right after the installation, but ==there are some settings that should be changed==... All the Settings are stored in the file **..\Microsoft Configuration Manager\AdminConsole\bin\RZ4CM.exe.config**. You can edit this file with a simple Text-Editor. And: make a Backup of the .config file, just in case of...

#### CM_SQLServer
To detect Application-Updates, it's required to configure SQL (read) access to the ConfigMgr-Database.
replace 'localhost' with the Name of the SQL Server Hosting the ConfigMgr DB:
` <setting name="CM_SQLServer" serializeAs="String">
   <value>localhost</value>
 </setting>`

#### CM_SQLServerDBName
Defines the name of the ConfigMgr Database. Normally it's CM_{SiteCode}:
` <setting name="CM_SQLServerDBName" serializeAs="String">
  <value>CM_TST</value>
 </setting>`

#### LimitingUserCollectionID
Contains the CollectionID of the limiting-Collection for all user-based collections. The Default SMS00003 is the ID for "**All User Groups**":
` <setting name="LimitingUserCollectionID" serializeAs="String">
  <value>SMS00003</value>
 </setting>`

#### CreateDeployments
RZ4CM automaticall creates deployment. Set this value to **False** if you want to create deployments manually.
`  <setting name="CreateDeployments" serializeAs="String">
  <value>True</value>
 </setting>`

#### DPGroup
Let the Content be replicated automatically to a specific Distribution-Point group. If the group does not exist, the content of the Application will not be synced to any DP's.
` <setting name="DPGroup" serializeAs="String">
  <value>All DP's</value>
 </setting>`

#### DefaultADGroup
RZ4CM will create a User-base Collection per default. You can specify an AD Group that is automatically member of the Collection. The default value "%USERDOMAIN%\Domain Users" may be great for a Demo Environment but in production, you should change this value to an AD-Group that contains only test users.
**Note:** All the Users of this Group will see the Software in Software-Center. All deployments to User-Collections will be set to "**Available**".

**Important**: If you leave this Field empty, a Device Collection will created (without any Members) and the deployment will be set to "**Required**".

If you want to have an empty User-Collection, specify an AD group that does not exists.
![](/content/images/2017/06/UserColl.png)
` <setting name="DefaultADGroup" serializeAs="String">
  <value>%USERDOMAIN%\Domain Users</value>
 </setting>`

#### OSRequirements
To automatically set the requirements for the target OS, you can specify (remove the unwanted OS's from the default) the OS requirements. In this case, the Application will only run on Windows 10.
` <setting name="OSRequirementsX64" serializeAs="Xml">
  <value>
   <ArrayOfString xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <string>Windows/All_x64_Windows_10_and_higher_Clients</string>
   </ArrayOfString>
  </value>
 </setting>
 <setting name="OSRequirementsX86" serializeAs="Xml">
  <value>
   <ArrayOfString xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <string>Windows/All_x86_Windows_10_and_higher_Clients</string>
   </ArrayOfString>
  </value>
 </setting>`

#### PrimaryUserRequired
If you don't want that the User must be a primary-user to get the Application, set the value to **False**.
` <setting name="PrimaryUserRequired" serializeAs="String">
  <value>False</value>
 </setting>`

#### CM_Server
The tool must know the name of the ConfigMgr Site Server. If you install RZ4CM on a remote System, specify the name of the Primary Site Server.
` <setting name="CM_Server" serializeAs="String">
  <value>localhost</value>
 </setting>`

#### CMContentSourceUNC
The Default Value ( \\localhost\admin$ ) will work everywhere but it does not make sense. Change the value to a share where RZ4CM can store the content of the Applications.
` <setting name="CMContentSourceUNC" serializeAs="String">
  <value>\\SCCM01\WKSSources$\Applications\RuckZuck_Apps</value>
 </setting>`

#### CMSecurityScope
If you have different security scopes, you can define the name of the scope where the Apllications are assigned to.
` <setting name="CMSecurityScope" serializeAs="String">
  <value>Workstation Mgmt</value>
 </setting>`

#### UpdateExistingApplications
Allows to update an existing Application in case of a change in the RuckZuck-Repository (e.g. Icon or description changes). 
Note: If the Product-Version changes, a new Application will be created instead of updating the existing App.
` <setting name="UpdateExistingApplications" serializeAs="String">
  <value>True</value>
 </setting>`

#### CollectionFolder
To create User- or Device Collection in a specific Sub-Folder, define a Folder-Name here 
` <setting name="CollectionFolder" serializeAs="String">
  <value>RuckZuck</value>
 </setting>`

#### AppFolder
Contains the name of a folder that contains all the created Applications
` <setting name="AppFolder" serializeAs="String">
  <value>RuckZuck</value>
 </setting>`

### Troubleshooting
If something goes wrong, RZ4CM will create a Log File in "**%TEMP%\RZError.txt**". 

Tips:  

- Check if the User running the tool has the required rights in CM
- To get updates of existing Apps, the User must have read access on the SQL DB ( View: v_ConfigurationItems )
