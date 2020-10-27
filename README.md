Azure SQL Database: Vertically Scale
====================================

            

The objective of this runbook is to demonstrate how Azure Automation can be used to schedule vertical scaling of a service object for a given database.  The motivating scenario is being able to scale-up a database to an S2 service objective during business
 hours and scale down to an S1 service objective during the evening hours.  With hour billing on Azure SQL Database, such scenarios enable significant cost savings.


 


_______________________________________________________________________________________________________________________________


(2/27/2015) With the release of the Azure SQL Database V12 preview/GA, there is a temporary
inconsistency between the current version of [Azure PowerShell](http://azure.microsoft.com/en-us/documentation/articles/install-configure-powershell/) and the version of Azure PowerShell hosted in Azure Automation.  Please
 note that this problem only occurs when connecting to a V12 SQL Database. As a result, when the script attempts to get the server context '$CTX = New-AzureSqlDatabaseServerContext -ManageUrl $SqlServerName -Credential $ServerCredential' the following error
 is returned:


   New-AzureSqlDatabaseServerContext : https://srvname.database.windows.net/v1/ManagementService.svc/GetAccessToken does not exist.


To fix the problem, we are going to upload the newest Azure PowerShell cmdlets to Azure Automation and override the existing (and V12
incompatible) Azure PowerShell cmdlets.  To make this fix, please follow the steps below:


1) Ensure that you have the latest version of [Azure PowerShell](http://azure.microsoft.com/en-us/documentation/articles/install-configure-powershell/) installed, go to Microsoft Azure PowerShell and type 'get-module azure'.  Your version should be at least 0.8.14 or greater.


2) Go to: C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\ServiceManagement


3) Copy the Azure folder to your desktop


4) Because Azure Automation has a 30MB upload limit for Assets, we're going to need to slim down the Azure PowerShell cmdlet folder.  Depending on your other use cases, the choice of what to remove may vary.  For the purposes of this example, I'll
 remove the HDInsight content.


5) In the Azure folder, first delete the 'HDInsight' folder


6) Next, open the Azure.psd1 file and delete any line that contains 'HDInsight' and save the file.


7) Zip the Azure folder.


8) In your Azure Automation Account, click on 'Assets' and then 'Import Module'.


9) Upload the Azure.zip folder from your desktop and override the existing module. 


10) Once the upload has completed successfully, you will be able to run against both versions of SQL Database.



Again, this fix will only be temporary until Azure Automation is refreshed with the next version of Azure PowerShell.  I apologize for the inconvenience.


 

 

        
    
TechNet gallery is retiring! This script was migrated from TechNet script center to GitHub by Microsoft Azure Automation product group. All the Script Center fields like Rating, RatingCount and DownloadCount have been carried over to Github as-is for the migrated scripts only. Note : The Script Center fields will not be applicable for the new repositories created in Github & hence those fields will not show up for new Github repositories.
