![Qlik](./media/image1.png)

# **_Configuring Qlik Composes with Azure Databricks_**

## **Partner Engineering**

<br>
<br>
<br>
<br>
John Park<br>
Principal Solution Architect<br>
john.park@qlik.com

![](./media/image2.png)

**Version: 1.1**<br>
**Initial Release Date: 17-Feb-20**<br><br><br><br>

**Revisions**      | **Notes**   | **Date**  | **Version**
------------------ | ----------- | --------- | -----------
Initial Draft      | 20-Feb-2020 | John Park | 0.1         |
Review of Language | 20-Feb-2020 | John Park | 0.2         |
Final Edit for V1  | 21-Feb-2020 | John Park | 1.0         |
Edits for Markdown | 25-Feb-2020 | John Park | 1.1         |

--------------------------------------------------------------------------------

# Table of Contents

[**Summary**](#summary)

[**Section A - Configuring Azure Components**](#section-a---configuring-azure-components)

[**Section B - Configuring Qlik Compose for Data Lakes**](#section-b---configuring-qlik-compose-for-data-lakes)

[**Section C - Connecting Qlik Compose with Qlik Replicate**](#section-c-connecting-qlik-compose-with-qlik-replicate)

## **Summary**

This document was created to supplement Qlik Compose Documentation for customers intending to use Qlik Compose with Azure Databricks. The Official Documentation can be found at

- <https://help.qlik.com/en-US/compose/Content/Compose/Home.htm>

## **Section A - Configuring Azure DatabricksComponents**

**High Level Overview**

- Create or Configure Azure Data Lake Storage (ADLS) Gen2  
_Log into Azure Portal and Create Storage Account with ADLSv2 Hierachical Storage Enabled._

- Create Azure Active Directory Application  
_Using Azure Active Directory register and the account for access on thrird party tools._

- Create ADLS File System and Folder  
_Using Storage Explorer setup containers filesystems and folders._


_The Detailed Instruction can be found in Accompanying Documentation ![Configuring Replicate with Azure Databricks]//Place Link ***_

## **Section B - Configuring Qlik Compose for Data Lakes**

**High Level Overview**

· Create or Configure Compose Project Using Databricks

· Create Landing and Storage

At this point Qlik Compose is setup and license are put in ready to go. You should see the following:

**_Figure B.0.1_**

![](./media/image3.jpeg)   
Qlik Compose for Data Lakes is ready start. Click "Add New Project"

**_Figure B.0.2_**

![](./media/image4.png)

Click on Databricks as Engine for this Project.

**_Figure B.0.3_**

![](./media/image5.png)

We will configure landing and storage areas of Data lake to Use ADLSv2 and Databricks Delta.

For Clarification Landing is where the Data is landed or ingested via a mechanism such as Qlik Replicate and Storage is are for Managed Data Lake. (Please refer the Official Online help)

**_Figure B.0.4_**

![](./media/image6.jpg)

For Storage are we will use a different container inside ADLSv2 container. For our case we logged on to Azure portal and created a new container inside storage account called "compose".

**_Figure B.0.5_**

![](./media/image7.jpg)

Inside Container "compose" we created set of Directories, for this Tutorial we will the directory "test".

**_Figure B.0.6_**

![](./media/image8.jpg) Using the following code, we are able to create compose_test are for Storage. (Please refer to Configuring Replicate with Azure Databricks Doc for more detail)

**_Figure B.0.7_**

![](./media/image9.jpg)

We validate by going to Azure Databricks portal and verifying under "Data" Tab

**_Figure B.0.8_**

![](./media/image10.jpg)

Configure Storage Connection

- Use the token generated for Databricks cluster.

- Use the Databricks cluster HTTP path from Cluster Definition

- For Host Server use Database Host Defined

- Create a new database for the storage connection in your Databricks Cluster.

**_Figure B.0.10_**

![](./media/image11.png)    
Click on Database name and if Databricks and Azure ADLSv2 is setup correctly you should see following.

**_Figure B.0.11_**

![](./media/image12.png)

If the '**...'** button does not bring up Select Schema windows please verify Databricks JDBC Driver is installed in C:\Program Files\Attunity\Compose for Data Lakes\java\jdbc Directory

If No Schema is available, please create a database and mount in Azure Databricks Notebook.

If setup was successful storage connection will validate in Qlik Compose window.

**_Figure B.0.11_**

![](./media/image13.jpg)

At this point Compose will need to configure a "Landing" zone where data will need to be landed. Landing are in Qlik Compose term is where raw data is landed.

Put a name for your Landing Zone select your Database name in Azure Databricks Delta and Test Connection. In this tutorial we used Qlik Replicate to land the Baseball data from MySQL to Databricks.(Please refer to Document: Tutorial Qlik Replicate with Azure Databricks)

**_Figure B.0.12_**

![](./media/image14.jpg)

**_Figure B.0.13_**

![A screenshot of a social media post Description automatically
generated](./media/image15.jpg) At this point Qlik Compose is ready to crate Data Marts using Azure Databricks Delta.

You can follow up this Configuration guide with a Tutorial Guide to see how to create a Sample Data Mart using Azure Databricks and Delta.

## **Section C -- Connecting Qlik Compose with Qlik Replicate**

Optional -- If you completed the "Tutorial Qlik Replicate with Azure Databricks"

Qlik Replicate and Qlik Compose can be Associated by doing the Following

Click Manage Replicate Servers

**_Figure C.1_**

![](./media/image16.jpeg)   
Add Replicate Servers

**_Figure C.2_**

![](./media/image17.jpg)

**_Figure C.3_**

![](./media/image18.jpg)  
Check Associate with Replicate task and Select your Replicate Task

**_Figure C.4_**

![](./media/image19.png)   
Click on "Test Connection" to verify Connection

**_Figure C.5_**

![](./media/image20.png)
