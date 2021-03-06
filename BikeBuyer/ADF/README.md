# This is the scenario where the Data Engineering is being done using Azure Data Factory Data Flow and Azure Databricks

![Original Data Scientist Work](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/deWithAzureDataFactoryDF.png)

## Upload Source Files to Blob

Create a Blob containers

![CreateContainer](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/createContainer.png)

Create one called `sourcedata`

![containerSourceData](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/containerSourceData.png)

Create another called `outputdata`

![containerOutputData](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/containerOutputData.png)

Uploaded the `adworks-bike-purchases.csv` and `potential-bike-buyers.csv` files into the sourcedata container in a folder called `bikes`.  You can find the files in the cloned repo at the path MLonBigData\BikeBuyer\LoyaltyCardBuyers

![uploadFilestobikes](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/uploadFilestobikes.png)

Here is the `bikes` folder

![blobSourceData](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/blobSourceData.png)

Here are the files inside of the `bikes` folder

![blobSourceDataFiles](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/blobSourceDataFiles.png)

## Author an Azure Data Factory Data Flow

In your Resource group click on the Data factory (V2) and then click on Author & Monitor

![authorAndMonitorDataFactory](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/authorAndMonitorDataFactory.png)

Click on the pencil (Author) icon

![clickPencil](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/clickPencil.png)

Click on Connections on the bottom of the left navigation panel.  Click on `+ New`

![createConnections](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/createConnections.png)

Click on the Data Store Linked Service and select `Azure Blob Storage`.  Click on `Continue`

![blobLinkedService](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/blobLinkedService.png)

Type in a name (like `AzureBlobStorage`) for the new Linked Service.  Choose AutoResolveIntegrationRuntime, Use Account key, Connection String, From Azure subscription, select your Azure Subscription, and Storage account name.  Click `Test` and then `Finish`

![blobLinkedServiceCfg1](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/blobLinkedServiceCfg1.png)

### Generate New Token for Azure Databricks Cluster

In your Resource group click on the Azure Databricks Service and the launch the Azure Databricks Workspace by clicking on the `Launch Workspace` button in the Azure Portal

![launchWorkspace](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/launchWorkspace.png)

Go to User Settings

![userSettings](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/userSettings.png)

Click `Generate New Token`

![generateNewToken](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/generateNewToken.png)

Create a Comment (Like `ADF Data Flow`) to identify the token.  Click `Generate`

![launchWorkspace](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/generateNewTokenDialog.png)

Copy and paste the token somewhere safe to keep.  You will need it in a bit to configure a Databricks Compute Linked Service.  Click `Done`

![copyToken](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/copyToken.png)

You can now see the created token

![tokenCreated](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/tokenCreated.png)

Return to ADF Connections on the bottom of the left navigation panel.  Click on `+ New`  
Click on the Compute Linked Service and select `Azure Databricks`.  Click on `Continue`

![databricksLinkedService](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/databricksLinkedService.png)

Type in a name for the new Linked Service.  Choose AutoResolveIntegrationRuntime, From Azure subscription, Azure Subscription, Databricks Workspace, and New job cluster.

![databricksLinkedServiceCfg1](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/databricksLinkedServiceCfg1.png)

Select Access token, and paste in the Access token you created above.  Choose a Cluster version, Cluster node type, Python Version, and Fixed or Outscaling worker option. Click `Test` and then `Finish`

![databricksLinkedServiceCfg2](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/databricksLinkedServiceCfg2.png)

You should now see your two new Linked Services

![connectionsCreated.](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/connectionsCreated.png)

### Create Datasets

Click the + next to Filter Resources to add new Factory Resources and select `Dataset`

![addNewFactoryResource1](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/addNewFactoryResource.png)

Select Azure Blob and click `Continue`

![newBlobDataset1](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/newBlobDataset.png)

Choose the format type of your data.  Select DelimitedText and click `Continue`

![newBlobDatasetFormat](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/newBlobDatasetFormat.png)

Name the dataset `Purchases`, select the AzureBlobStorage Linked service.  Browse to the `adworks-bike-purchases.csv` file in sourcedata\bikes.  Check the box `First row has header`. Import schema From connection/store and click `Continue`.  

![datasetPurchases](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/datasetPurchases.png)

It should look like this

![datasetPurchases2](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/datasetPurchases2.png)

Click the + next to Filter Resources to add new Factory Resources and select `Dataset`

![addNewFactoryResource1](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/addNewFactoryResource.png)

Select Azure Blob and click `Continue`

![newBlobDataset1](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/newBlobDataset.png)

Choose the format type of your data.  Select DelimitedText and click `Continue`

![newBlobDatasetFormat](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/newBlobDatasetFormat.png)

Name the dataset `PotentialBuyers`, select the AzureBlobStorage Linked service.  Browse to the `potential-bike-buyers.csv` file in sourcedata\bikes.  Check the box `First row has header`. Import schema From connection/store and click `Continue`. 

![datasetPotentialBuyers](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/datasetPotentialBuyers.png)

It should look like this

![datasetPurchases2](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/datasetPotentialBuyers2.png)

Click the + next to Filter Resources to add new Factory Resources and select `Dataset`

![addNewFactoryResource1](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/addNewFactoryResource.png)

Select Azure Blob and click `Continue`

![newBlobDataset1](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/newBlobDataset.png)

Choose the format type of your data.  Select DelimitedText and click `Continue`

![newBlobDatasetFormat](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/newBlobDatasetFormat.png)

Name the dataset `OutputDataBikes`, select the AzureBlobStorage Linked service.  Enter a file path of `outputdata/bikes`.  The file has not been created yet so you can't point to anything. Check the box `First row has header`. Import schema None and click `Continue`.   

![datasetOutputDataBikes](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/datasetOutputDataBikes.png)

It should look like this

![datasetOutputDataBikes2](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/datasetOutputDataBikes2.png)

Click on `Publish All`

![datasetsPublish](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/datasetsPublish.png)

### Create Data Flow

Click the + next to Filter Resources to add new Factory Resources and select `Data Flow`.

![addNewFactoryResource1](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/addNewFactoryResource.png)

Click Finish

![Finish](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/finish.png)

Click in the Add Source box.  Type `Buyers` as the Output stream name, and select `PotentialBuyers` as the Source Dataset

![buyersDataflow1](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/buyersDataflow1.png)

Click in the Add Source box.  Type `Purchases` as the Output stream name, and select `Purchases` as the Source Dataset

![purchasesDataflow1](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/purchasesDataflow1.png)

Click on the + sign in front of the `Buyers` Source and select `Join`

![joinDataflow1](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/joinDataflow1.png)

Type `JoinLoyaltyCardID` as the Output stream name, Left stream should default to `Buyers`, select `Purchases` for Right stream, Join type Full outer, and Join conditions `LoyaltyCardID == LoyaltyCardID` 

![joinDataflow2](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/joinDataflow2.png)

Click on the + sign in front of the `JoinLoyaltyCardID` Join and select `Select`

![selectDataflow1](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/selectDataflow1.png)

Name the Output stream name `SelectColumns` and select the 13 columns in the image below.  You may want to delete the columns that you don't use.  You can always add or Re-map the columns if you make a mistake.

![selectDataflow2](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/selectDataflow2.png)

Click on the + sign in front of the `SelectColumns` Select and select `Derived Column`

![derivedColumnDataflow1](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/derivedColumnDataflow1.png)

Name the Output stream name `BikesTrans` and add the following 8 columns and use these formulas:

`BikesTrans` --> `iif(isNull(Bikes), 0, 1)`

`MaritalStatusTrans` --> `iif(MaritalStatus == 'S', 1, 2 )`

`GenderTrans` --> `iif(Gender == 'M', 1, 2)`

`EducationTrans` --> `case(Education == 'High School', 1, Education == 'Partial College', 2, Education == 'Partial 1', 3, Education == 'Bachelors', 4, Education == 'Graduate Degree', 5, 0)`

`CommuteDistanceTrans` --> `case(CommuteDistance == '0-1 Miles', 1, CommuteDistance == '1-2 Miles', 2, CommuteDistance == '2-5 Miles', 5, CommuteDistance == '5-10 Miles', 10, 0)`

`RegionTrans` --> `case(SelectColumns@Region == 'United States', 1, SelectColumns@Region == 'Canada', 1, SelectColumns@Region == 'Germany', 2, SelectColumns@Region == 'France', 2, SelectColumns@Region == 'United Kingdom', 2, SelectColumns@Region == 'Australia', 3, 0)`

`AgeTrans` --> `toInteger(toInteger((currentDate() - toDate(BirthDate)))/365)`

`BikeBuyerTrans` --> `iif(isNull(Bikes ), 0, 1)`

![derivedColumnDataflow2](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/derivedColumnDataflow2.png)

Click on the + sign in front of the `BikesTrans` Dervived Column and select `Sink`

![sinkDataflow1](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/sinkDataflow1.png)

Name the Output stream name `BikeOutput` and chose `OutputDataBikes` Sink Dataset

![sinkDataflow2](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/sinkDataflow2.png)

Map the Input to the following Outputs

![sinkMap](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/sinkMap.png)

Click on `Publish All`

![publishAll](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/publishAll.png)


### Create Pipeline

Click the + next to Filter Resources to add new Factory Resources and select `Pipeline`.  

![addNewFactoryResource1](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/addNewFactoryResource.png)

Drag the Data Flow activity from the Move and Transform grouping to the palette on the right

![activitiesDF](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/activitiesDF.png)

Use exiting Data Flow and select an Existing Data Flow (one you just created).  Click Finish.

![activitiesDFexistingDF](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/activitiesDFexistingDF.png)

Publish All and then click `Trigger Now`

![triggerNow](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/triggerNow.png)

Click `Finish`

![triggerNowFinish](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/triggerNowFinish.png)

Click on the Monitor (Orange Cirlce) icon in the left pannel to monitor the job.

![monitor](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/monitor.png)

Once the pipeline succeeds check to see if the files are in Blob.  If it errors please troubleshoot.

![outputFilesBlob](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/outputFilesBlob.png)


### Add Sink to Azure SQL Database to Data Flow

Click on the + sign in front of the `BikesTrans` Dervived Column and select `Sink`

![sinkDataflow1](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/sinkDataflow1.png)

Name the Output stream name `BikeOutputSQL` and click +New next to Sink dataset (This is neccessary because we did not create the Azure SQL DB dataset earlier)

![sinkPlusNew](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/sinkPlusNew.png)

Choose Azure SQL Database and click Continue

![azureSQLdbdataset](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/azureSQLdbdataset.png)


Name it `BikeOutputSQL` and clink on the Linked service drop down and select +New

![azureSQLdbDatasetLinkedService1](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/azureSQLdbDatasetLinkedService1.png)

Select Connection String and choose From Azure Subscription.  Choose the Azure Subscription, Server name, Database name, SQL Authentication, and enter the User name and password for the SQL Database you created during the deployment. Test the Connection and click Finish. 

![azureSQLdbDatasetLinkedService2](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/azureSQLdbDatasetLinkedService2.png)

Select Create new table with `dbo` schema and `BikeOutputSQL` table name.
Click Finish

![azureSQLdbDatasetLinkedService3](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/azureSQLdbDatasetLinkedService3.png)

Click on `Publish All`
You might need to correct the RegionTrans column in the `BikesTrans` Dervived Column.

Click `Trigger Now`

![triggerNow](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/triggerNow.png)

Click `Finish`

![triggerNowFinish](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/triggerNowFinish.png)

Click on the Monitor (Orange Cirlce) icon in the left pannel to monitor the job.

![monitor](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/monitor.png)

Once the pipeline succeeds check to see if the table has been loaded in Azure SQL database.  You might want to install [Azure Data Studio](https://docs.microsoft.com/en-us/sql/azure-data-studio/download) which is a cross-OS-platform (Linux, Mac, Window) tool to connect to your database.

Here is how to build a connection in Azure Data Studio.  You can get the Server Name for Azure SQL database in the Azure Portal under Overview area.

![azureDataStudioConnection](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/azureDataStudioConnection.png)

Here is the query.  You should get 18484 rows.

![azureDataStudioQuery](https://raw.githubusercontent.com/DataSnowman/MLonBigData/master/images/azureDataStudioQuery.png)

