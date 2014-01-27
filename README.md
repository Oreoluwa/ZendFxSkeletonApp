ZendFxSkeletonApp
=================

Skeleton App using Zend Framework for Windows Platform


#Description:

1. manifest.xml, parameters.xml : These files are required to build a web deployable package for Windows Web platform Installer,
Azure websites installer and Windows Azure pack websites installer
2. Zend2 Directory : The source code here is  built using the original Zend framework's skeleton application: https://github.com/zendframework/ZendSkeletonApplication and using composer(https://getcomposer.org/) to include Zend framework
3. web.config : The web.config file allows you to run Zend framework on IIS , IIS Express and Azure (Public/Private Cloud)


#How is this different from orginal Zend Skeleton App :

These files are not part of the original Zend skeleton application:
   - manifest.xml
   - parameters.xml
   - web.config
   - Zend2/vendor/zendframework
   - Zend2/vendor/ZF2
   - Zend2/vendor/bin
   - Zend2/vendor/composer
    

#Installation on IIS :

Download the Zip file from https://github.com/AppHubBuild/ZendFx-WWAG-Package  and use Web deploy to deploy the application to local IIS 
http://www.iis.net/learn/publish/using-web-deploy/import-a-package-through-iis-manager 


#Installation on Azure Websites:

- Create a new Website on Windows Azure through the management portal (http://manange.windowsazure.com)
- Once the site is created, click on the site to access its Dashboard. Now download the Publish Profile. Here is a sample :

`<publishProfile profileName="myazuresite - Web Deploy" publishMethod="MSDeploy"` `publishUrl="waws-prod-001.publish.azurewebsites.windows.net:443" `
`msdeploySite="myazuresite" userName="$myazuresite" userPWD="7nbQc4ngW"` `destinationAppUrl="http://myazuresite.azurewebsites.net"`
`SQLServerDBConnectionString="" mySQLDBConnectionString="Database=SiteA3nnQoL6k0;Data` `Source=us-cdbr-azure-west-b.cleardb.com;User Id=b00ea5f73c603a;Password=somePassword" hostingProviderForumLink=""` `controlPanelLink="http://windows.azure.com">`

- Create a setparameters.xml file 
Build a SetParameters File for deploying your application
You can name this file as you would name a filename, but for the sake of this article let’s say the file is called SetParameters.xml .

This is a sample of the SetParameters.xml file for a Wordpress Package. You can see the parameters.xml file from the web deployable package here  .

`<parameters>`
`<setParameter name="AppPath" value="myazuresite" />`
`<setParameter name="DbServer" value="us-cdbr-azure-west-b.cleardb.com" /> `
`<setParameter name="DbName” value="SiteA3nnQoL6k0" /> `
`<setParameter name="DbUsername" value="b00ea5f73c603a" /> `
`<setParameter name="DbPassword" value="somePassword" /> `
`<setParameter name="DbAdminUsername" value="b00ea5f73c603a" /> `
`<setParameter name="DbAdminPassword" value="somePassword" /> `
`</parameters>`

###Note:

The name attribute for each <setParameter>  in the SetParameters file must match a parameter in the parameters.xml file present in web deployable package
The value attribute for each <setParameter> in the SetParameters file must be set.
If your application requires Database information, enter the database information of your remote database here. In the above example  , I have used a remote  MySQL database  and enter the information in this file
SetACL parameters should not be included in setparameters.xml file
Add a setparameter for any user define parameters in the parameters.xml file of your ZIP package 
 e.      Run Web Deploy Command Line
Open Command Prompt with elevated privileges and run msdeploy.exe to publish the application to Azure site you just created.

###Usage:

 `Msdeploy.exe –verb:sync –source:package=<pathToLocalZip>` `-dest:auto,computername=”<publisherURLwithHttpsAndPortOr8172>/msdeploy.axd?site=<remoteSiteName>”,`
 `username=<publishingUserName>,password=<publisherPwd>,authtype=basic  - setParamFile:<pathToLocalSetParamFile>`

###Example:

 `C:\Program Files\IIS\Microsoft Web Deploy V3>Msdeploy.exe -verb:sync -source:package="c:\packages\MyApplication.zip"` `-dest:auto,computername='https://waws-prod-001.publish.azurewebsites.windows.net:443/msdeploy.axd?site=myazuresite',`
 `username='$myazuresite',password='7nbQc4ngW',authtype=basic -setParamFile:"c:\setparameters.xml"`

Once your application package has been successfully deployed to Azure site. Browse the Site URL and to verify if the application was successfully deployed. 


