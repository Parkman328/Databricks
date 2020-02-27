![](./media/image1.png)  


# **_Tutorial Qlik Replicate and Azure Databricks_**

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
Initial Draft      | 03-Jan-2020 | John Park | 0.1         |
Additional Changes | 14-Feb-2020 | John Park | 0.2         |
Final Edit for V1  | 21-Feb-2020 | John Park | 0.4         |
Cosmetic Changes   | 21-Feb-2020 | John Park | 1.0         |
Edits for Markdown | 25-Feb-2020 | John Park | 1.1         |

# Table of Contents
--------------------

[**Summary**](#summary)

[**Section A - Configure/Verify MySQL Database**](#section-a---configureverify-mysql-database)

[**Part 1 - Verify MySQL Database**](#part-1---verify-mysql-database)

[**Part 2 - Create Sample Schema and Load Data**](#part-2---create-sample-schema-and-load-data)

[**Part 3 - Create and Configure Qlik Replicate Connection for MySQL DB**](#part-3---create-and-configure-qlik-replicate-connection-for-mysql-db)auto- 

[**Section B - Configure/Verify Azure Databricks**](#section-b---configureverify-azure-databricks)

[**Part 1 - Verify Azure Databricks**](#part-1---verify-azure-databricks)

[**Part 2 - Create/Verify Azure ADLS 2 and Databricks Connection**](#part-2---createverify-azure-adls-2-and-databricks-connection)

[**Section C - Create Qlik Replicate CDC Job from MySQL to Azure Databricks**](#section-c---create-qlik-replicate-cdc-job-from-mysql-to-azure-databricks)

[**Part 1 - Create Qlik Replicate CDC Job**](#part-1---create-qlik-replicate-cdc-job)

[**Part 2 - Test Initial Load and Verify Data Movements to Azure Databricks**](#part-2---test-initial-load-and-verify-data-movements-to-azure-databricks)

[**Part 3 - Test Changes and Verify Delta Movements to Azure Databricks**](#part-3---test-changes-and-verify-delta-movements-to-azure-databricks)


## **Summary**
---------------

This document was created to supplement Qlik Replicate Documentation for customers intending to Qlik Replicate and Azure Databricks. The Office
Documentation can be found at

- <https://help.qlik.com/en-US/replicate/Content/Replicate/Home.htm>.

This Tutorial should help Customers interested in a step by step by
directions to implement Qlik Replicate CDC process from RDBMS to Azure
Databricks.

**High Level Overview**

-   Create Connection to MySQL Database

-   Create Connection to Azure Databricks

-   Create Qlik Replicate CDC Job

-   Verify Data Movement and Changes

High Level Architecture

![](./media/image3.png)

**Prerequisites for this this Tutorial are following:**

1.  Working Azure Account with ability to access to resources

2.  Installed Qlik Replicate Server version 6.5 or above

3.  Configured and verified MySQL Database

4.  Configured and verified Azure Databricks Account

5.  Connectivity between MySQL Database, Qlik Replicate Server and Azure Databricks instances.

# **Section A - Configure/Verify MySQL Database**
------------------------------------------------

### **Part 1 - Verify MySQL Database**

For this tutorial We have setup a MySQL 8.0.19 CE Database Sample
Database

> *Note: For this Tutorial to work MySQL must **enable binary logging**
and use **mysql_native_password** for authentication.*

Verify Database Connectivity Using MySQL Workbench.

***Figure A.1.0***

![A screenshot of a cell phone Description automatically
generated](./media/image4.png)

### **Part 2 - Create Sample Schema and Load Data**

The Data we will use will be from Baseball Data from
<http://www.seanlahman.com/baseball-archive/statistics/>

Download the MySQL Version of Data and Execute

Note the Baseball Data Version we are using is 2019 data and it must be
loaded to MySQL Version 8.0

***Figure A.2.0***

![A screenshot of a social media post Description automatically
generated](./media/image5.jpeg)

Once the Import has finished you should be able inspect the database and
see tables, and results from sample query.

***Figure A.2.1***

![A screenshot of a social media post Description automatically
generated](./media/image6.jpeg)


 **Sample Query**
```
use lahmansbaseballdb;

select * from people; 
```

### **Part 3 - Create and Configure Qlik Replicate Connection for MySQL DB**

Now we will create Qlik Replicate connections for Source and verify the
connection. Qlik Replicate should be installed and functional.

> **_For this document we are using Attunity Replicate Version 6.5.0.354._**

After login on the first thing we need to do is create a source
endpoint. We do this by clicking the Manage Endpoint Connections button
at the top of the screen.

***Figure A.3.0***

![Manage Endpoints
Image](./media/image7.png)

Following Prompt will appear.

***Figure A.3.1***

![Add New Endpoint
Image](./media/image8.png)

From there, click on "*Add New Endpoint Connection"* link or the + New
Endpoint Connection button at the top of the screen.

Once you do that you will see this window:

***Figure A.3.2***

![](./media/image9.png)

We will now create a MySQL source endpoint:

-   Replace the text **New Endpoint Connection 1** with something more
    descriptive like MySQL -Source Local,

-   Verify Source radio button is selected,

-   Select "MySQL" from the dropdown selection box

Fill in the blanks as indicated in the images above:

-   Server: ```[your_servername]```

-   Port: ```3306```

-   Username: ```[your_username]```

-   Password: ```[your_password]```

-   Security/SSL Mode: **Required** (Use Down Arrow to View Menu)

***Figure A.3.2***

![A screenshot of a cell phone Description automatically
generated](./media/image10.jpeg)

Click on Test Connection. Your screen should look like the following,
indicating that your connection succeeded.

Assuming so, click Save and the configuration of your MySQL source
endpoint is complete. Click Close to close the window.

For more details about using MySQL as a source, please review the
appropriate section in the User Guide [Using a MySQL-Based Database as a
Source](https://tdxxuxjzmt-vm.testdrive.attunity.com/files/AttunityReplicate_User_Guide.pdf#%5B%7B%22num%22%3A4258%2C%22gen%22%3A0%7D%2C%7B%22name%22%3A%22XYZ%22%7D%2C79.5%2C741%2C0%5D)

# **Section B - Configure/Verify Azure Databricks**
---------------------------------------------------

### **Part 1 - Verify Azure Databricks**

First, we need to set up the Azure storage account that Qlik Replicate
will use to map data into Databricks. We will setup Azure Data Lake
Storage (ADLSv2) to manage the external tables.

Please refer to "Configuring Azure ADLSvs2" for Detail instructions.

### **Part 2 - Create/Verify Azure ADLS 2 and Databricks Connection**

Note Azure Databricks Cluster should be live and Azure ADLSv2 setting
should be configured property.

Following Information should be gathered

**Azure Storage:**

-   Storage Account Name: ___________ (Azure Portal -->
    Storage Account)

-   Azure Active Directory ID: ___________ (Azure Portal -->
    Azure Active Directory --> App Registration)

-   Azure Active Directory application ID: ___________ (Azure
    Portal --> Azure Active Directory -- App Registration)

-   Azure Active Directory application key: ___________
    (Azure Portal --> Azure Active Directory --> App Registration -->
    Certificates and Secret)

-   File System: ___________ (Azure Portal --> Storage
    Account -> Containers)

-   Target folder: ___________ (Azure Portal --> Storage
    Account -> Containers)

***Figure B.2.0***

![A screenshot of a cell phone Description automatically
generated](./media/image11.jpg)
**Databricks ODBC Access:**

-   Host: ___________ (Azure Databricks Portal --> Clusters
    -> Advanced Options -> JDBC/ODBC)

-   Port: ___________ (Azure Databricks Portal --> Clusters
    -> Advanced Options -> JDBC/ODBC)

-   Token: ___________ (Azure Databricks Portal --> User
    Settings -> Access Tokens -> Generate New Token)

-   HTTP Path: ___________ (Azure Databricks Portal -->
    Clusters -> Advanced Options -> JDBC/ODBC

-   Database: ___________ (Azure Databricks Portal --> Data)

-   Mount Path: ___________ (Azure Databricks Portal ->
    Notebook)

***Figure B.2.1***

![](./media/image12.jpg)

Click on Test Connection. Your screen should look like the following,
indicating that your connection succeeded.

***Figure B.2.2***

![](./media/image13.jpg)



# **Section C - Create Qlik Replicate CDC Job from MySQL to Azure Databricks**
-----------------------------------------------------------------------------

### **Part 1 - Create Qlik Replicate CDC Job**

Now that we have configured our MySQL source and Azure Databricks target
endpoints, we need to tie them together in what we call a Replicate
**task**. In short, a task defines the following:

-   A source endpoint

-   A target endpoint

-   The list of tables that we want to capture

-   Any transformations we want to make on the data

To get started, we need to create a task. Click on the + New Task button
at the top of the screen.

***Figure C.1.0***

![](./media/image14.png)
Once you do, a window like this will pop up:

***Figure C.1.1***

![](./media/image15.png)

Give this task a meaningful name like MySQL-to-Azure Databricks. For
this task we will take the defaults:

-   Name: MySQL-to-Azure Databricks

-   Unidirectional

-   Full Load: enabled (Blue highlight is enabled; click to enable /
    disable.)

-   Apply Changes: enabled (Blue highlight is enabled; click to enable /disable.)

-   Store Changes: disabled (Blue highlight is enabled; click to enable
    / disable.)

***Figure C.1.2***

![](./media/image16.jpeg)

Once you have everything set, press OK to create the task. When you have
completed this step, you will see a window that looks like this:

***Figure C.1.3***

![A screenshot of a social media post Description automatically
generated](./media/image17.jpg)

Attunity is all about ease of use. The interface is point-and-click,
drag-and-drop. To configure our task, we need to select a source
endpoint (MySQL) and a target endpoint (Azure Databricks). You can
either drag the MySQL Source endpoint from the box on the left of the
screen and drop it into the circle that says Drop source endpoint here,
or you can click on the arrow that appears just to the right of the
endpoint when you highlight it.

Repeat the same process for the Azure Databricks Target endpoint. Your
screen should now look like this:

When you have finished dropping the Source Endpoints it should look like
the following:

***Figure C.1.4***

![A screenshot of a social media post Description automatically
generated](./media/image18.jpeg)

Our next step is to select the tables we want to replicate from MySQL
into Azure Databricks. Click on the *Table Selection...* button in the
top center of your browser.

***Figure C.1.7***

![A picture containing screenshot Description automatically
generated](./media/image19.jpeg)

and from there select the ***lahmansbaseballdb*** schema.

![A screenshot of a cell phone Description automatically
generated](./media/image20.jpg)

Press the ***Search*** button. This will retrieve a list of all the
tables in the *lahmanbasebald* schema. Note: entering % is not strictly
required. By default, Attunity Replicate will search for all tables (%)
if you do not limit the search.

***Figure C.1.8***

![A screenshot of a cell phone Description automatically
generated](./media/image21.jpeg)

and then press the >> button to move all of the tables from the
Results list into the Selected Tables list. Note that we also had the
option of simply wildcarding all tables, or selectively choosing tables
from the Results list.

***Figure C.1.9***

![A screenshot of a cell phone Description automatically
generated](./media/image22.jpeg)

Click "OK" and you job is ready to execute.

Click "Save" Icon

***Figure C.1.10***

![A screenshot of a social media post Description automatically
generated](./media/image23.jpeg)

Press "Run" Icon.

***Figure C.1.11***

![A screenshot of a social media post Description automatically
generated](./media/image24.jpeg)

### **Part 2 - Test Initial Load and Verify Data Movements to Azure Databricks**

At this step you should be able to execute the Task from Replicate and
verify the Data from Azure Databricks window.

Below is a what you should see on Qlik Replicate Task Manager

***Figure C.2.0***

![A screenshot of a cell phone Description automatically
generated](./media/image25.jpeg)

Below is what you see on Azure Databricks Database by Clicking Data
button and Clicking on Tables.

***Figure C.2.1***

![A screenshot of a cell phone Description automatically
generated](./media/image26.jpeg)


***Figure C.2.2***

![A screenshot of a computer Description automatically
generated](./media/image27.jpeg)

### **Part 3 - Test Changes and Verify Delta Movements to Azure Databricks**

Make Changes to your MySQL Database and see changes flow through to
Azure Databricks Database.

Alter Player Table by Adding new Player

```
INSERT INTO `lahmansbaseballdb`. `people` (`playerID`, `birthYear`,  `birthMonth`,  `birthDay`,  `birthCountry`,`birthState`,  `birthCity`,  `deathYear`,  `deathMonth`, `deathDay`, `deathCountry`, `deathState`, `deathCity`, `nameFirst`, `nameLast`, `nameGiven`, `weight`, `height`, `bats`, `throws`, `debut`, `finalGame`, `retroID`,`bbrefID`, `birth_date`, `debut_date`, `finalgame_date`, `death_date`) 
VALUES ('parkjo01', '1980', '3', '2', 'USA','GA', 'Lilburn', '2043', '03', '2', 'USA', 'FL','Ocala', 'John', 'Park', 'John Park', '205', '73', 'R','R', '1995-09-10', '1987-09-27', 'parkj101', 'parkjo01','1980-03-02', '1987-09-10', '1995-09-27', '2043-11-15')
```
Verify the Changes in Attunity Task Monitor 1 row has been updated.

***Figure C.3.0***

![A screenshot of a cell phone Description automatically
generated](./media/image28.jpeg)

Verify the Changes in Azure Databricks.

***Figure C.3.1***

![A screenshot of a cell phone Description automatically
generated](./media/image29.jpeg)