![Qlik](./media/image1.png){width="3.2083333333333335in"
height="0.9791666666666666in"}

Configuring Qlik Composes with Azure Databricks

Partner Engineering

**Version: 1.0**

**Initial Release Date: 17-Feb-20**

![](./media/image2.png){width="2.90625in" height="1.875in"}

  **Revisions**                                                         **Notes**                          **DATE**
  --------------------------------------------------------------------- ---------------------------------- -----------
  John Park/Author/ Principal Solution Architect, Partner Engineering   Initial Draft                      03-Jan-20
  John Park                                                             Additional Changes for Draft 1.0   14-Feb-20
  John Park                                                             Removing Azure ADLS Setup          18-Feb-20
  John Park                                                             Cosmetic Changes                   18-Feb-20

Table of Contents {#table-of-contents .TOCHeading}
=================

[Summary 4](#summary)

[Section A - Configuring Azure Components
5](#section-a---configuring-azure-components)

[Section B - Configuring Qlik Compose for Data Lakes
5](#section-b---configuring-qlik-compose-for-data-lakes)

[Section C -- Connecting Qlik Compose with Qlik Replicate
11](#section-c-connecting-qlik-compose-with-qlik-replicate)

 **Summary **
-------------

This document was created to supplement Qlik Compose Documentation for
customers intending to use Qlik Compose with Azure Databricks. The
Official Documentation can be found at
<https://help.qlik.com/en-US/compose/Content/Compose/Home.htm>

**Section A - Configuring Azure Components**
--------------------------------------------

**High Level Overview** 

·      Create or Configure Azure Data Lake Storage (ADLS) Gen2 

·      Create Azure Active Directory Application 

·      Create ADLS File System and Folder 

The Detailed Instruction can be found in Accompanying Documentation
"Configuring Replicate with Azure Databricks"

**Section B - Configuring Qlik Compose for Data Lakes**
-------------------------------------------------------

**High Level Overview** 

·      Create or Configure Compose Project Using Databricks 

·      Create Landing and Storage

At this point Qlik Compose is setup and license are put in ready to go.
You should see the following:

***Figure B.0.1***

![A screenshot of a cell phone Description automatically
generated](./media/image3.jpeg){width="3.246988188976378in"
height="1.6480391513560806in"}

Qlik Compose for Data Lakes is ready start. Click "Add New Project"

***Figure B.0.2***

![A screenshot of a cell phone Description automatically
generated](./media/image4.png){width="3.3674693788276464in"
height="2.623403324584427in"}

Click on Databricks as Engine for this Project.

***Figure B.0.3***

![A screenshot of a cell phone Description automatically
generated](./media/image5.png){width="4.145833333333333in"
height="3.401466535433071in"}

We will configure landing and storage areas of Data lake to Use ADLSv2
and Databricks Delta.

For Clarification Landing is where the Data is landed or ingested via a
mechanism such as Qlik Replicate and Storage is are for Managed Data
Lake. (Please refer the Official Online help)

***Figure B.0.4***

![A screenshot of a cell phone Description automatically
generated](./media/image6.jpg){width="4.7088604549431325in"
height="2.290036089238845in"}

For Storage are we will use a different container inside ADLSv2
container. For our case we logged on to Azure portal and created a new
container inside storage account called "compose".

***Figure B.0.5***

![A screenshot of a cell phone Description automatically
generated](./media/image7.jpg){width="5.849394138232721in"
height="1.444851268591426in"}

Inside Container "compose" we created set of Directories, for this
Tutorial we will the directory "test".

***Figure B.0.6***

![A screenshot of a cell phone Description automatically
generated](./media/image8.jpg){width="5.722889326334208in"
height="1.4331681977252844in"}

Using the following code, we are able to create compose\_test are for
Storage. (Please refer to Configuring Replicate with Azure Databricks
Doc for more detail)

***Figure B.0.7***

![A screenshot of a cell phone Description automatically
generated](./media/image9.jpg){width="6.072290026246719in"
height="1.9430030621172354in"}

We validate by going to Azure Databricks portal and verifying under
"Data" Tab

***Figure B.0.8***

![A screenshot of a cell phone Description automatically
generated](./media/image10.jpg){width="2.198794838145232in"
height="1.7526935695538057in"}

Configure Storage Connection

-   Use the token generated for Databricks cluster.

-   Use the Databricks cluster HTTP path from Cluster Definition

-   For Host Server use Database Host Defined

-   Create a new database for the storage connection in your Databricks
    Cluster.

***Figure B.0.10***

![A screenshot of a cell phone Description automatically
generated](./media/image11.png){width="3.4879516622922133in"
height="3.6947692475940506in"}

Click on Database name and if Databricks and Azure ADLSv2 is setup
correctly you should see following.

***Figure B.0.11***

![A screenshot of a cell phone Description automatically
generated](./media/image12.png){width="2.596385608048994in"
height="1.8321719160104988in"}

If the '**...'** button does not bring up Select Schema windows please
verify Databricks JDBC Driver is installed in C:\\Program
Files\\Attunity\\Compose for Data Lakes\\java\\jdbc Directory

If No Schema is available, please create a database and mount in Azure
Databricks Notebook.

If setup was successful storage connection will validate in Qlik Compose
window.

***Figure B.0.11***

![A screenshot of a cell phone Description automatically
generated](./media/image13.jpg){width="2.268767497812773in"
height="2.457831364829396in"}

At this point Compose will need to configure a "Landing" zone where data
will need to be landed. Landing are in Qlik Compose term is where raw
data is landed.

Put a name for your Landing Zone select your Database name in Azure
Databricks Delta and Test Connection. In this tutorial we used Qlik
Replicate to land the Baseball data from MySQL to Databricks.(Please
refer to Document: Tutorial Qlik Replicate with Azure Databricks)

***Figure B.0.12***

![A screenshot of a cell phone Description automatically
generated](./media/image14.jpg){width="2.412995406824147in"
height="2.5903608923884516in"}

***Figure B.0.13***

![A screenshot of a social media post Description automatically
generated](./media/image15.jpg){width="5.433734689413823in"
height="2.7011931321084863in"}

At this point Qlik Compose is ready to crate Data Marts using Azure
Databricks Delta.

You can follow up this Configuration guide with a Tutorial Guide to see
how to create a Sample Data Mart using Azure Databricks and Delta.

**Section C -- Connecting Qlik Compose with Qlik Replicate**
------------------------------------------------------------

Optional -- If you completed the "Tutorial Qlik Replicate with Azure
Databricks"

Qlik Replicate and Qlik Compose can be Associated by doing the Following

Click Manage Replicate Servers

***Figure C.1***

![A screenshot of a cell phone Description automatically
generated](./media/image16.jpeg){width="3.0361450131233596in"
height="1.4476476377952756in"}

Add Replicate Servers

***Figure C.2***

![A screenshot of a cell phone Description automatically
generated](./media/image17.jpg){width="1.8184678477690288in"
height="1.6807228783902013in"}

***Figure C.3***

![A screenshot of a cell phone Description automatically
generated](./media/image18.jpg){width="3.150602580927384in"
height="1.9250317147856517in"}

Check Associate with Replicate task and Select your Replicate Task

***Figure C.4***

![A screenshot of a cell phone Description automatically
generated](./media/image19.png){width="2.746988188976378in"
height="2.980893482064742in"}

Click on "Test Connection" to verify Connection

***Figure C.5***

![A screenshot of a cell phone Description automatically
generated](./media/image20.png){width="2.8072014435695536in"
height="3.0420352143482066in"}
