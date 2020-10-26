## System Requirements:
* ~~Visual Studio 2017~~ Visual Studio 2019 [Visual Studio Download](https://visualstudio.microsoft.com/downloads/)
* Visual Basic 6.0 (if editing COM+ or legacy apps) [Visual Basic 6.0 dowload for Windows 10](https://www.microsoft.com/en-us/download/details.aspx?id=5721)
* SQL Server Management Studio (version ~~?~~ 18.7) [SSMS 18.7 download link](https://aka.ms/ssmsfullsetup)

## Installation Instructions:
> Note: it is important that you do the first 2 step before cloning the repo (or large binary files may not work correctly)
* download and install git: https://git-scm.com/downloads
  * you should use version `??` + (or run "`?????`")

* download and install git-lfs: https://git-lfs.github.com/
  * open "git bash" (windows search) Another excellent cli replacement is cmder can be installed using chocolatey. 
    * [Chocolatey Package Manager for Windows](https://chocolatey.org/)
    * [Cmder](https://chocolatey.org/packages/Cmder)
  * at the "$" prompt, type; `git lfs install`, and press enter.

* clone repo 
```cmd
   // open command prompt
   // create working directory
   mkdir c:\git // example
   cd c:\git
   git clone https://github.com/ma-dev-global/ma.git
```

### Local System Setup
The following is for setting up a locally run solution from your desktop.
* setup machine.config files 
  * open ALL of your local machine.config files (need to be in "admin" mode to make changes)
    > NOTE: YOU SHOULD MAKE BACKUPS FIRST (JUST IN CASE)
    * C:\Windows\Microsoft.NET\Framework\v2.0.50727\Config\machine.config _(may not be present)_
    * C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config\machine.config
    * C:\Windows\Microsoft.NET\Framework64\v2.0.50727\Config\machine.config _(may not be present)_
    * C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config
  * add the following `<section>`, `<connectionStrings>` and `<maSecurity>` information into the `<configuration>` node
```xml
<configuration>
	<configSections>
		<section name="maSecurity" requirePermission="false" type="System.Configuration.NameValueSectionHandler, System, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"/>
		<!-- 
		... 
		-->
	</configSections>	
	<!-- 
	... 
	-->	
	<connectionStrings>
		<add name="ManageAmerica" connectionString="data source=YOUR_DATABASE_ADDRESS; Initial Catalog=Disaster; User ID=YOUR_DB_USER; Password=YOUR_DB_PASSWORD;" providerName="System.Data.SqlClient"/>
		<add name="MainServer" connectionString="data source=YOUR_DATABASE_ADDRESS; Initial Catalog=Disaster; User ID=YOUR_DB_USER; Password=YOUR_DB_PASSWORD;" providerName="System.Data.SqlClient"/>
		<add name="RemoteServer" connectionString="data source=YOUR_DATABASE_ADDRESS; Initial Catalog=Disaster; User ID=YOUR_DB_USER; Password=YOUR_DB_PASSWORD;" providerName="System.Data.SqlClient"/>
		<add name="HitsEntities" connectionString="metadata=res://*/Context.HITS.csdl|res://*/Context.HITS.ssdl|res://*/Context.HITS.msl;provider=System.Data.SqlClient;provider connection string=&quot;data source=YOUR_DATABASE_ADDRESS;initial catalog=Hits;persist security info=True;user id=YOUR_DB_USER;password=YOUR_DB_PASSWORD;MultipleActiveResultSets=True;App=EntityFramework&quot;" providerName="System.Data.EntityClient"/>    
		<add name="ManageAmericaMAReportEF" connectionString="data source=YOUR_DATABASE_ADDRESS;initial catalog=maReports;persist security info=True;User ID=YOUR_DB_USER; Password=YOUR_DB_PASSWORD;MultipleActiveResultSets=True;App=EntityFramework" providerName="System.Data.SqlClient" />
		<add name="ManageAmericaDisasterEF" connectionString="data source=YOUR_DATABASE_ADDRESS;initial catalog=Disaster;persist security info=True;User ID=YOUR_DB_USER; Password=YOUR_DB_PASSWORD;MultipleActiveResultSets=True;App=EntityFramework" providerName="System.Data.SqlClient" />
		<add name="ManageAmericaMAFinanceEF" connectionString="data source=YOUR_DATABASE_ADDRESS;initial catalog=maFinance;persist security info=True;User ID=YOUR_DB_USER; Password=YOUR_DB_PASSWORD;MultipleActiveResultSets=True;App=EntityFramework" providerName="System.Data.SqlClient" />
		<add name="ManageAmericaCoreEF" connectionString="data source=YOUR_DATABASE_ADDRESS;initial catalog=Core;persist security info=True;User ID=YOUR_DB_USER; Password=YOUR_DB_PASSWORD;MultipleActiveResultSets=True;App=EntityFramework" providerName="System.Data.SqlClient" />
		<add name="ManageAmericaHangFire" connectionString="data source=YOUR_DATABASE_ADDRESS;initial catalog=Hangfire;persist security info=True;User ID=hfprocq; Password=YOUR_DB_PASSWORD;MultipleActiveResultSets=True;App=EntityFramework" providerName="System.Data.SqlClient" />

	</connectionStrings>	
	<maSecurity>
		<add key="MAKey" value="Your 32 Byte Encryption Key Here" />
		<add key="MASqlUser" value="YourDbUser" />
		<add key="MASqlPw" value="YourDbPasswordHere" />
	</maSecurity>
	<appSettings>
        	<add key="ManageAmericaWebServerUrl" value="https://localhost:PORT_NUMBER/" />
	        <add key="ManageAmericaDocumentCachePath" value="C:\Temp\Cache\" />
		<add key="MaApiPassword" value="USER_PASSWORD" />
        	<add key="ManageAmericaReportErrorEmail" value="YOUR_EMAIL_ADDRESS" />
		<add key="Environment" value="ANY_VALUE" />
        </appSettings>
</configuration>
```
* setup system DSNs (odbc 32-bit)
  * open ODBC Data Source Administrator (32-bit)
    * In Windows ~~7~~ 10, navigate to: `%systemdrive%\Windows\SysWoW64` folder and run `Odbcad32.exe`
         * You can also hit the `Windows Key + r` and type `Odbcad32.exe`
  * select "System DSN" tab
  * create "SQL Server" DSN's for the following (with sql server authentication):
    * Core - point to "`core`" db on your dev server _(address ?)_, using your user/password
    * Disaster - point to "`disaster`" db on your dev server, using your user/password
    * maDataPump - point to "`maFinance`" db on your dev server, using your user/password
* get the latest "APPS" packages (com+, Interop/Gac and optionally, installer files... if you need more functionality) and place on the "D" drive under: "`D:\Apps`" _What if you don't have a D drive?_
  > You will need a "D" drive anyway, as there are multiple hardcoded drive paths in the code!... for now. 
  > Eventually we will create nuGet packages for these, but for now, You can pull them from _(what is the dev server path?)_ DEV
  * COM/GAC/INTEROP - all in "`/MA_PACKAGES/current`"
    * copy "`/MA_PACKAGES/current/apps`" -> "`D:\Apps`"
      * COM+: `/MA_PACKAGES/current/Apps/COM+/`
      * GAC: `/MA_PACKAGES/current/Apps/Dot.Net/GAC/`
      * INTEROP: `/MA_PACKAGES/current/Apps/Dot.Net/Interop/`
  * INSTALLERS (if you need them) - pull directly "`dev/Apps`": 
    * copy "`/Apps/Installers`" -> "D:\Apps\Installers" (**_note: not from the packages area_**)
* run "`D:\Apps\Dot.Net\Interop\MaCOM\RegisterMACOM.bat`" (as admin)
* run "`D:\Apps\Dot.Net\GAC\SecurityCryptography\Security.Cryptography.Install.bat`" (as admin)
  * If you are on Win7 (wtf man?) 
    * Traverse to directory `D:\Apps\Dot.Net\GAC\SecurityCryptography`. 
    * Drag and drop file `Security.Cryptography.dll` to directory `C:\Windows\assembly`. 
* setup com+
  * auto setup (TBD):
    * (todo: nuGet... coming soon)
  * manual setup:
    * open "Component Services" Hit `Windows Key + r` and type `Component Services`
    * create an empty COM+ Application (Server Application) and call it "`MADEVCOM`"
      * ![Select COM+ Applications](https://github.com/smhoff/NotesFromSetup/blob/main/images/complus1.png)
      * Set application identity to user Your user
      * User "CreaterOwner"
      * open properties and Traverse to the Security tab
        * Uncheck "Enforce access checks for this application"
        * Select "Perform access checks at the process and component level."
    * Add new components from "`D:\Apps\COM+\`"
      * run "D:/Apps/com+/unregLocalCOMS.bat" as admin (wait for "pause..." to close)      
      * run "D:/Apps/com+/regLocalCOMS.bat" as admin (wait for "pause..." to close)
* Other Installs (TBD)
  * AspTreeView from Apps/Installers


# Projects/Solutions

The "`ManageAmerica.sln`" the main solution and should contain everything needed to run the site locally. Multiple solutions can be created if their focus is limited (or independent of) the main solution. However, any project that is dependent upon the compiled assembly of another project *MUST* be included in the project to ensure continuity.

All web pages housed within the main WebSite, should used the "CodeFile" attribute with their code file along side. Additionally, web pages (and their "source" files) should originate in the WebSite that will host them. The same goes for any other project (including other WebApp projects). 

**DO NOT** create a separate WebApp and use a build-script to "_COPY_" files to their destination _host_ site

## config files
Several files are intentionally excluded from the repo. These are file that would be different depending on the server or environment hosting the application. Each excluded file has a file counterpart that **will be** included in the repo (but not used in the solution). These all end with the .example extension. The files should be copied/pasted to their parent directories and edited per your system requirements.

The following is a list of currently "excluded" files that may be "required" depending on the solution/project you are running.

* \AcapExtract.Console\Config\nlog.config.example
* \AutoDial.Console\App.config.example
* \Billing.Console\Config\*.config.example
* \Eft.IntelliPay.Returns.Generator\Config\*.config.example
* \Eft.Posting.Verification\Config\nlog.example.config
* \Lockbox.Export\Config\appSettings.example.config
* \MA.COM.App\app.config.example - _only if overriding machine.config_
* \MA.COM.RMS.Tests\App.config.example
* \MA.Transfer.Tests\App.config.Example
* \MA.WebUI\web.config.example
* \Origen.ESign.Processor\Config\nlog.example.config
* \PayLease.Init\Config\*.config.example
* \ProfitStars.Export\Config\nlog.example.config
* \ProfitStars.Import\Config\nlog.example.config
* \PubApi\Config\*.config.example
* \StandAlone\LoginChecker\App.config.example
* \Swistak.Export\Config\appSettings.example.config
* \VB6\COM\maFinance\Export\Settings\ExportReports.ini.example
* \VB6\COM\PropertyUpload\serverList.txt.example
* \WebSite\Config\*.config.example
* \WebUI\web.config.example

## set framework version to 4.7.2
Make sure to install Framework 4.7.2 in Visual Studio if it is not installed. To do that follow these steps:
1. Search for Visual Studio Installer and open that.  Under "More" choose "Modify".
2. Click on ".NET desktop development" on the left and expand "ASP.NET and web development" on the right.  Under "Optional" check ".NET Framework 4.7.2 development tools".  Then click the "Modify" button.

Within the Website project in the MA solution in Visual Studio, change your LOCAL version of the website/config/compilation.confg file to use 4.7.2:
```xml
<compilation debug="true" targetFramework="4.7.2">
```

## Setup IIS Express Cert
* follow Resolution #2 here: https://blogs.msdn.microsoft.com/robert_mcmurray/2013/11/15/how-to-trust-the-iis-express-self-signed-certificate/ 
<details><summary>Instructions from the above url (in case it gets taken down):</summary>
<p>
Resolution Number #2 - Configure your computer to trust the IIS Express Certificate
A more-detailed approach is to configure your computer system to trust the IIS Express certificate, and you might want to do this if your computer is shared by several developers who log in with their individual accounts. To configure your computer to trust the IIS Express certificate, use the following steps:

1. Open a blank Microsoft Management Console by clicking Start, then Run, entering "mmc" and clicking OK:
   * Note: You can also open a blank Microsoft Management Console by typing "mmc" from a command prompt and pressing the Enter key.
2. Add a snap-in to manage certificates for the local computer:
   * Click File, and then click Add/Remove Snap-in:
   * When the Add or Remove Snap-ins dialog box is displayed, click Certificates, and then click Add:
   * When the Certificates Snap-ins dialog box is displayed, click Computer account, and then click Next:
   * Click Local computer, and then click Finish:
   * Click OK to close the Add or Remove Snap-ins dialog box:

3. Export the IIS Express certificate from the computer's personal store:
   * In the Console Root, expand Certificates (Local Computer), then expand Personal, and then click Certificates:
   * Select the certificate with the following attributes:
      * Issued to = "localhost"
      * Issued by = "localhost"
      * Friendly Name = "IIS Express Development Certificate"
   * Click Action, then click All Tasks, and then click Export:
   * When the Certificate Export Wizard is displayed, click Next:
   * Click No, do not export the private key, and then click Next:
   * Click DER encoded binary X.509 (.CER), and then click Next:
   * Enter the path for exported certificate, e.g. "c:\users\robert\desktop\iisexpress.cer", and then click Next:
   * Click Finish to export the certificate:
   * Click OK when the Certificate Export Wizard displays a dialog box informing you that the export was successful:

4. Import the IIS Express certificate to the computer's Trusted Root Certification Authorities store:
   * In the Console Root, expand Certificates (Local Computer), then expand Trusted Root Certification Authorities, and then click Certificates:
   * Click Action, then click All Tasks, and then click Import:
   * When the Certificate Import Wizard is displayed, click Next:
   * Enter the path to your exported certificate, e.g. "c:\users\robert\desktop\iisexpress.cer", and then click Next:
   * Ensure that Place all certificates in the following store is checked and verify that the selected Certificate store is set to Trusted Root Certification Authorities, and then click click Next:
   * Click Finish to import the certificate:
   * Click OK when the Certificate Import Wizard displays a dialog box informing you that the import was successful:
   * You IIS Express certificate should now be displayed in the listed of Trusted Root Certification Authorities as "localhost":
</p>
</details>

## Running Solution
* Run VS as an Administrator
* Setup WebSite
  * right click on WebSite project and select "Properties Window"
    * Set "SSL Enabled" to True
    * Take note of the "SSL URL"
  * right click on WebSite project and select "Property Pages"
    * select "Start Options"
    * set "Start URL" to the "SSL url" for the site (above). e.g. "`https://localhost:44324/`"
* Setup Solution
  * right click on Solution and select "Properties"
  * set "Startup Project" to "Single startup project" -> "WebSite"
* Run Solution
  * when browser opens, it will warn about unverified ssl... go on.
  * When landing page loads, **STOP**
* Setup IIS Express (one time)
  * Open Windows Explore
  * navigate to your repo location
  * find the `.vs/config` folder and open it (also do this in `C:\Users\[your username]\Documents\IISExpress\config`)
  * open applicationhost.config 
    >  (for more info, see: https://stackoverflow.com/questions/4769751/how-to-set-allow-parent-paths-in-iis-express-config)
    * add or set the "`enableParentPaths`" attribute of the `<asp>` node under the `<system.webServer>` to "`true`" (copy all if you also want debuging). Modify the system.webServer under the `<configuration>` node NOT under the `<location>` node.:
    ```xml
    <configuration>
        <system.webServer>
            <serverRuntime />
            <asp enableParentPaths="true" appAllowDebugging="true" appAllowClientDebug="true" scriptErrorSentToBrowser="true">
                <session allowSessionState="true" />
                <cache diskTemplateCacheDirectory="%TEMP%\iisexpress\ASP Compiled Templates" />
                <limits />
            </asp>
            <!-- ... -->
        </system.webServer>
    </configuration>
    ```
    * Find and Modify the the "ExtensionlessUrl-Integrated-4.0" entry within the handlers node to set the attribute verb="*" (Under the `<location>` node):

    ```xml
    <add name="ExtensionlessUrl-Integrated-4.0" path="*." verb="*" type="System.Web.Handlers.TransferRequestHandler" preCondition="integratedMode,runtimeVersionv4.0" responseBufferLimit="0" />
    ```
  

* Return to browser and refresh

