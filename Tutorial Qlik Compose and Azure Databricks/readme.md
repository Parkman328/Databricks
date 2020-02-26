![Qlik](./media/image1.png){width="3.2083333333333335in"
height="0.9791666666666666in"}

Tutorial -- Qlik Compose and Azure Databricks with Delta

Partner Engineering

**Version: 1.0**

**Initial Release Date: 17-Feb-20**

![A close up of a logo Description automatically
generated](./media/image2.png){width="2.9166666666666665in"
height="1.875in"}

  **Revisions**                                                         **Notes**       **DATE**
  --------------------------------------------------------------------- --------------- ----------
  John Park/Author/ Principal Solution Architect, Partner Engineering   Initial Draft   18-Feb

  {#section .TOCHeading}
=

Table of Contents {#table-of-contents .TOCHeading}
=================

[Summary 4](#summary)

[Section A -- Verify Connections for Qlik Compose
5](#section-a-verify-connections-for-qlik-compose)

[**Part 1 -- Starting Point for this Tutorial**
5](#part-1-starting-point-for-this-tutorial)

[Section B -- Move Data from Landing to Storage Using Compose
6](#section-b-move-data-from-landing-to-storage-using-compose)

[**Part 1 -- Add New Entity** 6](#part-1-add-new-entity)

[**Part 2-- Create Tasks for Compose**
10](#part-2-create-tasks-for-compose)

[**Part 3 -- View Tasks from Monitor and Run Tasks**
14](#part-3-view-tasks-from-monitor-and-run-tasks)

[Section C -- Use Lookup and Created Curated Table for Data Lake
16](#section-2)

[**Part 1 -- Create Qlik Replicate CDC Job**
16](#part-1-create-a-new-entity-in-storage-zone)

[Section D - Verify Data Movement and Changes in Qlik Compose
16](#section-d---verify-data-movement-and-changes-in-azure-databricks)

[**Part 2-- Test Initial Load and Verify Data Movements to Azure
Databricks** 16](#_Toc33063797)

[**Part 3 -- Test Changes and Verify Delta Movements to Azure
Databricks** 16](#_Toc33063798)

 **Summary **
-------------

This document was created to supplement Qlik Compose Documentation for
customers intending to Qlik Replicate and Azure Databricks. The Office
Documentation can be found at
<https://help.qlik.com/en-US/replicate/Content/Replicate/Home.htm>.

This Tutorial should help Customers interested in a step by step by
directions to implement Qlik Compose for Data Lake to Automate Creating
a Managed Data Lake from Data Landed in Azure Databricks. This Tutorial
is continuation from

*Tutorial Qlik Replicate and Azure Databricks and Configuring Qlik
Compose with Azure Databricks. Please note this is a simple Tutorial of
Qlik Compose and we will not be delving into Details. *

High Level Overview

-   Verify Connections

-   Scenario 1 - Move Data from Landing to Storage using Compose

-   Scenario 2 - Use Lookup to Create a Player List with Hall Of Fame
    Inductees

-   Verify Data Movement and Changes in Qlik Compose

High Level Architecture

![image: Database-to-Delta-Lake
Replication](./media/image3.png){width="6.5in" height="1.4in"}

Prerequisites for this this Tutorial are following:

1.  Installed Qlik Compose Server version 6.5 or above

2.  Configured and verified Azure Databricks Account

**Section A -- Verify Connections for Qlik Compose**
----------------------------------------------------

### **Part 1 -- Starting Point for this Tutorial**

Qlik Compose should be installed properly and Landing Zone and Storage
Connection should be configured. Your Compose windows should look
similar to following.

***Figure A.0.1***

![A screenshot of a cell phone Description automatically
generated](./media/image4.jpeg){width="6.5in" height="2.93125in"}

Pressing the "Connections" Button bring up Landing and Storage
connections.

**Verify each connection by Pressing "Test Connection" Button.**

***Figure A.0.2***

![A screenshot of a cell phone Description automatically
generated](./media/image5.jpeg){width="3.837349081364829in"
height="3.0046937882764655in"}

**Section B -- Move Data from Landing to Storage Using Compose**
----------------------------------------------------------------

The next phase after you've streamed data into Azure Databricks is to
consider how to operationalize the setup, ongoing care, and feeding of
your data lake. In a typical data lake , most of the initial deployment
time and effort is spent writing ETL (Extract-Transform-Load) code
curate data for use by third party tools and to run advanced analytics.
Qlik Compose is a automation platform managed data lakes. The role of
Compose is to automates the time-consuming tasks of building and
maintaining data lakes. It's metadata-driven approach generates the
necessary artifacts and ETL scripts to automatically manage your entire
data lake lifecycle. As a result, business analysts and data architects
experience substantial productivity gains that speed the time to
insight.

For example, Data engineering, ingesting(hydrating) wrangling of a
typical data lake consumes about 50% of the initial deployment effort.
However, because Compose generates the appropriate ETL code, that data
lake is managed.

The first use case scenario we will walk through is simple movement of
data from Landing Zone to Storage area with metadata changes and
curating of columns.

High Level Overview

-   We will move tables people, teams, batting, and pitching to Storage.

-   We will enhance the metadata for batting table and add more
    descriptors.

### **Part 1 -- Add New Entity**

In Qlik Compose in the Storage tile Press "Metadata" Button

***Figure A.0.1***

![A screenshot of a cell phone Description automatically
generated](./media/image6.jpeg){width="3.95419072615923in"
height="3.1084339457567802in"}

You should see a full screen Manage Metadata window

***Figure A.0.2***

![A screenshot of a cell phone Description automatically
generated](./media/image7.jpeg){width="4.234939851268591in"
height="1.9858070866141733in"}

Press "Discover" Button

***Figure A.0.3***

![A screenshot of a cell phone Description automatically
generated](./media/image8.jpeg){width="4.315277777777778in"
height="2.3554221347331583in"}

Press the Search button to see all the tables from Landing Zone

***Figure A.0.4***

![A screenshot of a cell phone Description automatically
generated](./media/image9.jpeg){width="4.331325459317585in"
height="2.3493733595800523in"}

Move people, team, batting, and pitching to Selected Tables/View by
selecting one by one and using "\>" button.

Hit "OK" button to Generate Metadata from Landing to Storage

***Figure A.0.5***

![A screenshot of a cell phone Description automatically
generated](./media/image10.png){width="3.2756944444444445in"
height="2.4571259842519684in"}

At this point you are going to get a screen that looks like this.

***Figure A.0.6***

![A screenshot of a social media post Description automatically
generated](./media/image11.jpeg){width="4.114457567804025in"
height="1.9209590988626422in"}

We are going to use DataDictionary from Lahman's Baseball and Enhance
the Metadata and only pickup columns we deem necessary

Data Dictionary
<http://www.seanlahman.com/files/database/readme2019.txt>

For Batting Table Remove ID column and Enhance the Column Names

Step 1. Highlight "ID" and Press Delete button on Top.

Step 2. Double Click "playerID" and check Key

Step 3. Rename all Columns and Remove Unneeded Columns to Names from
above Dictionary

*ID, team\_id, G\_batting columns removed and " " replaced with \_*

The final Output should look like the following

***Figure A.0.7***

![A screenshot of a social media post Description automatically
generated](./media/image12.jpeg){width="3.9277110673665794in"
height="1.8371281714785652in"}

Click "Validate" Button

***Figure A.0.8***

![A screenshot of a cell phone Description automatically
generated](./media/image13.jpeg){width="3.825300743657043in"
height="2.0961504811898513in"}

For people, teams and pitching we will leave those tables alone.

### **Part 2-- Create Tasks for Compose**

After the steps above you should have added 4 entities and on the **Data
Storage Tasks** you should see full and CDC task autogenerated**. Using
auto Discovery Qlik Compose Creates task to move data from Compose to
Storage area using Delta.**

***Figure A.010***

![A screenshot of a cell phone Description automatically
generated](./media/image14.jpeg){width="6.5in"
height="2.7131944444444445in"}

Click on Data Storage Tasks and you can see the 2 Tasks and Mapping for
those tasks.

***Figure A.011***

![A screenshot of a social media post Description automatically
generated](./media/image15.jpeg){width="6.5in"
height="3.0368055555555555in"}

Double Click on batting mappings

***Figure A.012***

![A screenshot of a social media post Description automatically
generated](./media/image16.jpeg){width="6.5in"
height="3.0854166666666667in"}

Verify the Metadata update are complete, and all columns are mapped.

Press OK to go back to "Managed Data Storage Tasks" area.(Figure A.0.11)

Press Close to go to Main Screen. (Figure A.0.10)

Press "Create" Button to Initiate the Table Creations. ***Tables must be
created before tasks/Mappings can be validated an activated***

***Figure A.013***

![A screenshot of a cell phone Description automatically
generated](./media/image17.png){width="3.535673665791776in"
height="2.6445778652668417in"}

Press Close button when you see "The Storage Zone tables were created
successfully."

***Figure A.013***

![A screenshot of a cell phone Description automatically
generated](./media/image18.png){width="3.783132108486439in"
height="2.801376859142607in"}

In the Storage Zone Tile "Create" Button has now Turned into "Validate"
Button

***Figure A.014***

![A screenshot of a social media post Description automatically
generated](./media/image19.jpeg){width="4.867469378827646in"
height="2.105596019247594in"}

Press Validate to Validate Storage -- (Not Necessary but use to Sync
Storage with Mapping)

Press Button for "Data Storage Tasks" and Highlight all task and Press
"Generate Button"

***Figure A.015***

![A screenshot of a social media post Description automatically
generated](./media/image20.jpeg){width="4.457831364829397in"
height="2.089846894138233in"}

***Figure A.016***

![A screenshot of a cell phone Description automatically
generated](./media/image21.png){width="3.0372550306211723in"
height="2.2831332020997377in"}

Press Close and Generate for Task "Landing-Lahmansbaseball\_CDC" (Select
from Tasks List)

In the menu "Run" button and "Task Commands" button should now be
selectable.

***Figure A.017***

![A screenshot of a social media post Description automatically
generated](./media/image22.jpeg){width="6.5in"
height="3.045138888888889in"}

At this point you can run these tasks from this screen for this Tutorial
Press Close

### **Part 3 -- View Tasks from Monitor and Run Tasks**

At this point we have created 2 tasks, one for Full and One for CDC into
Databricks Delta where 4 tables (team, batting, people, pitching) are
able to be moved to Managed Data Lake on Azure Databricks.

4 table are referred to as 4 Entities

2 Tasks are shown and Task each have set of instructions.(Please refer
to Official Documentation for more detail)

This is what your Screen should look like

***Figure A.018***

![A screenshot of a social media post Description automatically
generated](./media/image23.jpeg){width="4.006023622047244in"
height="1.8908770778652668in"}

Press "Monitor" Icon in top right Corner

***Figure A.018***

![A screenshot of a social media post Description automatically
generated](./media/image24.jpeg){width="3.849397419072616in"
height="1.8457360017497813in"}

Note: Since my Qlik Replicate Server is Connected Qlik Compose Server I
can see my CDC process to Land the data into Landing Zone located.

Highlight "Landing-Lahmansbaseball" Task(Full) load and press "Run"

***Figure A.018***

![A screenshot of a cell phone Description automatically
generated](./media/image25.jpeg){width="4.194753937007874in"
height="1.9992300962379703in"}

This should be your output

***Figure A.018***

![A screenshot of a computer screen Description automatically
generated](./media/image26.jpeg){width="4.144577865266841in"
height="2.03465113735783in"}

***Figure A.019***

![A screenshot of a cell phone Description automatically
generated](./media/image27.jpeg){width="4.138554243219597in"
height="1.9525492125984252in"}

At this point you have created a Qlik Compose Storage area with 2 Tasks
that moved data from a **Landing Zone to Storage area. **

**Please refer to Documentation about Scheduling, Notification and
Workflows.**

**Section C -- Use Lookup and Created Curated Table for Data Lake**
-------------------------------------------------------------------

In this scenario you will create a new Entity in Qlik Compose where you
will generate a list of people and teams and lookup in hall of fame
table to place a marker if player was inducted to hall of fame. This
scenario is similar to using Compose to generate curated data set for
managed data lake.

### **Part 1 -- Create a New Entity in Storage Zone**

Click the "Metadata" Button in Storage Zone

Click "+New Entity" button

***Figure C.0.1***

![A screenshot of a social media post Description automatically
generated](./media/image28.jpeg){width="5.2831321084864395in"
height="2.485216535433071in"}

Add New Entity and use a name like "player\_hof"

***Figure C.0.2***

![A screenshot of a cell phone Description automatically
generated](./media/image29.png){width="3.686746500437445in"
height="1.7311165791776029in"}

Click on "player\_hof" and using the "+New Attribute" add the needed
attributes.

1.  "namefirst", "nameLast" and "playerID" can be added with lookup with
    Attribute Domain

    ***Figure C.0.3***

    ![A screenshot of a cell phone Description automatically
    generated](./media/image30.png){width="3.3220778652668415in"
    height="3.7891568241469815in"}

2.  Using the "+" button define new attributes "hof"

    ***Figure C.0.4***

    ![A screenshot of a cell phone Description automatically
    generated](./media/image31.jpeg){width="2.4924026684164478in"
    height="2.8912937445319336in"}

3.  Double Click on "playerID" and check the "Key" checkbox.

4.  Press "Validate" button and everything should validate.

This is what your "Manage Metadata" screen should look like

***Figure C.0.5***

![A screenshot of a cell phone Description automatically
generated](./media/image32.jpeg){width="6.5in"
height="3.0284722222222222in"}

Go to Main Screen and Validate Storage Zone

![A screenshot of a social media post Description automatically
generated](./media/image33.jpeg){width="6.5in"
height="3.0909722222222222in"}

Press Validate and Let Qlik Compose adjust Storage. Press "Adjust
Automatically"

***Figure C.0.6***

![A screenshot of a social media post Description automatically
generated](./media/image34.jpeg){width="3.7570548993875765in"
height="1.7530118110236221in"}

***Figure C.0.7***

![A screenshot of a cell phone Description automatically
generated](./media/image35.png){width="3.470138888888889in"
height="2.57174978127734in"}

Press Data Storage Tasks

**Figure C.0.8**

![A screenshot of a social media post Description automatically
generated](./media/image36.jpeg){width="6.5in"
height="3.0479166666666666in"}

See that "player\_hof" table is available without mapping.

Add New Task by Pressing "+ New Task"

Name Task something unique.

Create a new Task with no Change Processing since this is a Full Load
and no history needs to be kept.

**Figure C.0.9**

![A screenshot of a cell phone Description automatically
generated](./media/image37.jpeg){width="3.5367497812773405in"
height="3.578313648293963in"}

In the Mapping Area press "+ New Mapping"

**Figure C.0.10**

![A screenshot of a cell phone Description automatically
generated](./media/image38.jpeg){width="6.5in"
height="3.0597222222222222in"}

Edit Mapping Screen will appear.

![A screenshot of a cell phone Description automatically
generated](./media/image39.jpeg){width="6.5in"
height="3.066666666666667in"}

Bring in people Table and press "Auto-Map" playerID, namefirst, and
namelast will auto map.

![A screenshot of a social media post Description automatically
generated](./media/image40.jpeg){width="6.457983377077865in"
height="3.0652777777777778in"}

Press the Lookup Icon for 'hof'

![A screenshot of a cell phone Description automatically
generated](./media/image41.jpeg){width="3.9411767279090113in"
height="2.2985990813648294in"}

For hof use "halloffame" table as lookup and place "inducted" attribute
hof

![A screenshot of a cell phone Description automatically
generated](./media/image42.jpeg){width="2.57082895888014in"
height="2.3571423884514435in"}

Lookup Transformation should look like the following

![A screenshot of a cell phone Description automatically
generated](./media/image43.png){width="3.9989654418197724in"
height="4.2899157917760276in"}

Go to main screen and Validate Storage Zone again. This time Storage
zone will be different than Metadata. Generate Adjust Script.

![A screenshot of a cell phone Description automatically
generated](./media/image44.jpeg){width="3.34033573928259in"
height="1.5616786964129483in"}

![A screenshot of a social media post Description automatically
generated](./media/image45.jpeg){width="3.8963648293963256in"
height="2.1134448818897638in"}

Copy Adjust Script and execute in Azure Databricks Notes with needed
adjustments.

Click Validate and Resync

![A screenshot of a social media post Description automatically
generated](./media/image46.jpeg){width="6.5in"
height="3.073611111111111in"}

Go Into "Manage Data Storage Tasks" and Generate for "Load-Hall-Of-Fame"
Task

![A screenshot of a cell phone Description automatically
generated](./media/image47.png){width="6.5in"
height="4.788888888888889in"}

Go to Monitor and Execute "Load-Hall-Of-Fame" Task ![A screenshot of a
computer Description automatically
generated](./media/image48.jpeg){width="6.5in" height="3.4875in"}

At this point you have moved 5 Entities or DataSets into Managed Data
Lake.

**Section D - Verify Data Movement and Changes in Azure Databricks**
--------------------------------------------------------------------

By accessing Azure Databricks Portal you should validated following

-   Creation of Database 'compose\_test'

-   Creating of Table team, batting, people and pitching

-   Creation of Enhanced Metadata / Column Names for batting table

-   Creation player\_hof table and column hof populated properly

![A screenshot of a cell phone Description automatically
generated](./media/image49.jpeg){width="1.655462598425197in"
height="1.5767574365704287in"}

![A screenshot of a social media post Description automatically
generated](./media/image50.jpeg){width="4.794117454068242in"
height="2.259790026246719in"}

![A screenshot of a cell phone Description automatically
generated](./media/image51.jpeg){width="6.5in"
height="2.2583333333333333in"}
