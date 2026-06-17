# Exercise 1: Ingest data with a pipeline in Microsoft Fabric

### Estimated Duration: 90 Minutes

## 📘 Scenario

**Contoso Retail** has recently migrated its sales analytics platform to **Microsoft Fabric**. The analytics team needs a centralized solution to ingest, store, transform, and analyze sales transaction data for reporting purposes.

In this exercise, you will help Contoso set up a Fabric workspace, create a Lakehouse, ingest sales data using pipelines, transform the data with notebooks, query the processed data using SQL, and create visual reports for business analysis.

## 📖 Overview

In this exercise, you will use **Microsoft Fabric** to ingest and analyze sales data using a **Lakehouse architecture**. You will create a workspace and Lakehouse, build a data pipeline to load sales data into OneLake, transform the data using notebooks, query the processed data with SQL, and create basic visualizations and reports for analysis.

## 🎯 Objectives

In this exercise, you will be able to complete the following tasks:

- Task 1: Sign up for Microsoft Fabric Trial
- Task 2: Create a workspace
- Task 3: Create a lakehouse
- Task 4: Create a pipeline
- Task 5: Create a notebook
- Task 6: Use SQL to query tables
- Task 7: Create a visual query
- Task 8: Create a report

## Task 1: Sign up for Microsoft Fabric Trial

In this task, you will initiate your 60-day free trial of Microsoft Fabric by signing up through the Fabric app, providing access to its comprehensive suite of data integration, analytics, and visualization tools

1. On the **Power BI homepage**, click on the **Profile icon (1)** on the top right, and then click on **Start trial (2)**.

    ![Account-manager-start](./Images/01/E1T1S1.png)

1. On the **Activate your 60-day free Fabric trial capacity** dialog box, click **Activate**.  

    ![](./Images/01/E1T1S2-2005.png)

1. On the **Successfully upgraded to Microsoft Fabric** pop-up, click **OK**.

    ![](./Images/p1t1p3.png)

1. Close the **Invite teammates to try Fabric to extend your trial** pop-up.

    ![](./Images/p1t1p4.png)

1. On the **Power BI homepage**, click on the **Profile icon (1)** on the top right again, and verify **Fabric and Power BI trial status (2)**.

    ![Account-manager-start](./Images/01/E1T1S3-2005.png)

## Task 2: Create a workspace

In this task, you will create a Fabric workspace. The workspace will contain all the items needed for this lakehouse tutorial, which includes lakehouse, dataflows, Data Factory pipelines, notebooks, Power BI datasets, and reports.

1. On the left-hand pane of the Power BI portal, select **Workspaces (1)** and click on **+ New workspace (2)**.

    ![](./Images/p1t2p1.png)

1. On the **Create a workspace** page, enter the following details:

    - **Name:** Enter **fabric-<inject key="DeploymentID" enableCopy="false"/> (1)**

        > **Note:** The name may appear as a space between **fabric-** and the **unique ID**, but ideally, it does not have any spaces. Please make sure to remove any spaces while entering the name.

    - Expand the **Advanced (2)** section.

      ![name-and-desc-of-workspc](./Images/f3.png)

    - Select **License mode** as **Fabric (3)**.
    - From the dropdown list, select the available **Capacity (4)**.
    - Click **Apply (5)** to create and open the workspace.
 
      ![advanced-and-apply](./Images/01/E1T2S2ii.png)

1. On the **Introducing task flows** pop-up window, click **Got it**.

    ![](./Images/p1t2p3.png)

1. In the **Workspace**, select **Manage access** from the top menu.

     ![](./Images/01/E1T2S4-2005.png)

1. In the Manage access pane, **select + Add people or groups**

    ![](./Images/01/E1T2S5-2005.png)

1. In the **Add people** pane, paste the SPN `https://aec-svc/` **(1)**, then select the **Admin (2)**, and click **Add (3)**.

     ![](./Images/01/E1T2S6-2005.png)

1. You can close the Manage access window.

## Task 3: Create a Lakehouse

In this task, switch to the Data engineering experience and create a new Lakehouse. You'll use it to ingest and manage data in the following steps.

1. At the bottom left of the Power BI portal, select the **Power BI (1)** icon and switch to the **Fabric (2)** experience.

   ![](./Images/E1T3S1-i.png)

   ![](./Images/E1T3S1.1-1108.png)

1. On the **Welcome to the Fabric view** pop-up window, click **Cancel**.

    ![](./Images/p1t3p2.png)

1. In the left pane, navigate to **Workspaces (1)** icon and select **fabric-<inject key="DeploymentID" enableCopy="false"/> (2)**. 

    ![](./Images/p1t3p3.png)

1. Click on **+ New item (1)** to create a new lakehouse. In the search box, search for **Lakehouse (2)** and select **Lakehouse (3)** from the list.

    ![](./Images/01/E1T3S4-2005.png)

1. Enter the following details and click on **Create (4)**:

    - **Name**: **Lakehouse_<inject key="DeploymentID" enableCopy="false"/> (1)**. 

    - **Location:** **fabric-<inject key="DeploymentID" enableCopy="false"/> (2)**
    
    - **Lakehouse Scehmas:** **Unchecked (3)**
        
        > **Note:** Make sure to uncheck the Lakehouse schemas while creating the Lakehouse, as checking it would cause issues in the later exercises. 

        ![](./Images/01/E1T3S5-2005.png) 

1. On the **Lakehouse_<inject key="DeploymentID" enableCopy="false"/>** tab in the left pane, click the **Ellipsis (...) (1)** menu for the **Files** node, click on **New subfolder (2)**.
    
    ![](./Images/01/E1T3S6-2005.png)

1. Enter the Folder name as **new_data (1)** and click on **Create (2)**.

    ![](./Images/E1T3S6-1108.png)

## Task 4: Create a pipeline

In this task, you'll create a pipeline to automate data workflows. Using the Copy data activity, you'll extract data from a source and copy it to a file in the lakehouse, streamlining the data ingestion process.

1. From the left navigation pane, select **fabric-<inject key="DeploymentID" enableCopy="false"/> (1)** workspace, then click on same workspace **(2)** and then click on **+ New item (3)**.

    ![](./Images/e1p2t4p1.png)

    ![](./Images/p1t4p1(1).png)

1. In the search box, search for **pipeline (1)** and select **Pipeline (2)**.

    ![](./Images/01/E1T4S2-2005.png)

1. Create a new data pipeline named **Ingest Sales Data Pipeline (1)** and click on **Create (2)**. 
    
    ![](./Images/01/E1T4S3-2005.png)

    > **Note**: By Default the name of the pipeline is Pipeline_1, Please change it to as mentioned above.
   
1. On the **Build a data pipeline to organize and move your data** page, select **Copy data assistant (1)**.

   ![03](./Images/01/E1T4S4.png)

1. In the **Copy job** wizard, on the **Choose data source** page, search for **Http (1)** and select the **Http (2)** source from the results.

   ![](./Images/img-03.png)

1. In the **Connection settings** page, enter the following settings and click on **Next (5)**:
    
    - URL: **`https://raw.githubusercontent.com/MicrosoftLearning/dp-data/main/sales.csv` (1)**
    - Connection: **Create new connection (2)**
    - Connection name: **Connection<inject key="DeploymentID" enableCopy="false"/> (3)**
    - Authentication kind: **Anonymous (4)**
  
      ![](./Images/img-04.png)
    
1. On the **Choose data** page, keep the default settings and click **Next**.
    
    ![05](./Images/01/E1T4S7-2005.png)
   
1. Wait for the data to be sampled, then verify the following settings:

    > **Note:** Click anywhere on the Choose data window to enable the view. 

   - **File format:** DelimitedText **(1)**
   - **Column delimiter:** Comma (,) **(2)**
   - **Row delimiter:** Line feed (\n) **(3)**
   - Click **Preview data (4)** to view a sample of the data.
   - After reviewing, close the preview and click **Next (5)**.
  
      ![Account-manager-start](./Images/img-06.png)
    
      ![Account-manager-start](./Images/01/E1T4S8i.png)

1. On the **Choose data destination** page, click **OneLake catalog (1)** and select the lakehouse **Lakehouse\_<inject key="DeploymentID" enableCopy="false"/> (2)**.
    
    ![](./Images/01/E1T4S9-2005.png)

1. On the **Settings** page, select **Full copy (1)**. Under **Destination root folder**, select **Files (2)**, and then click **Next (3)**.

    ![](./Images/img-02.png)

1. On the **Map to destination** page, set the following data destination options.

   - Folder path: Click on **Browse (1)**, select **new_data (2)** and click **OK (3)**.
   - File name: **sales.csv (4)**

     ![08](./Images/01/E1T4S11a-2005.png)

     ![08](./Images/01/E1T4S11b-2005.png)

1. Expand the file format settings and select the following options:

   - File format: **DelimitedText (1)**
   - Expand the **File format settings**
   - Column delimiter: **Comma (,) (2)**
   - Row delimiter: **Line feed (\n) (3)**
   - Click **Next (4)**

     ![08](./Images/E1T1S11.png)

     ![08](./Images/01/E1T4S12.2-2005.png)

1. On the **Review + save** page, review the details of your copy operation and then select **Save**.

    ![08](./Images/01/E1T4S13-2005.png)

1. A new pipeline containing a **Copy job** activity is created, as shown here:

    ![Screenshot of a pipeline with a Copy Data activity.](./Images/01/E1T4S14-2005.png)

1. Click on the **Copy job (1)**, then select the **Settings (2)** tab, from the Connection dropdown **(3)**, select **Browse all (4)**.

    ![08](./Images/01/E1T4S15-2005.png)

1. On **Choose a data source to get started**, click on **Copy job**.

    ![08](./Images/01/E1T4S16-2005.png)

1. On the **Connect data source** page, click on **Sign in (1)** and select **<inject key="AzureAdUserEmail"></inject> (2)**. Then click **Connect (3)**.

    ![08](./Images/p1t4p10(6).png)

    ![08](./Images/p1t4p10(7).png)

    ![08](./Images/p1t4p10(8).png)

1. Under the **Home** tab, click on **Run**.

    ![08](./Images/01/E1T4S18-2005.png)

1. On the **Save and run?** pop-up, click **Save and run**.

    ![08](./Images/p1t4p10(10).png)

1. When the pipeline starts to run, you can monitor its status in the **Output** pane under the pipeline designer. Use the **&#8635;** (*Refresh*) icon to refresh the status, and wait until it has been successfully updated.

    ![Screenshot of a pipeline with a Copy Data activity.](./Images/01/E1T4S20-2005.png)

1. On the **Workspaces** page, select **fabric-<inject key="DeploymentID" enableCopy="false"/> (1)** and click **Lakehouse_<inject key="DeploymentID" enableCopy="false"/> (2)** to open the lakehouse.  

   ![](./Images/p1t4p10(12).png)

1. In the **Explorer** pane, expand **new_data (1)** and select **sales.csv (2)** to open the file to verify the data is loaded.  

   ![](./Images/p1t4p10(13).png)

## Task 5: Create a Notebook

In this task, you'll create a Notebook to document your data analysis. You'll set up the environment, import libraries, and structure your code for exploration, visualization, and insights.

1. Select **Workspaces (1)** on left hand side and select **fabric-<inject key="DeploymentID" enableCopy="false"/> (2)**.

    ![](./Images/e1t5s1.png)

1. Click on **+ New item (1)** In the New Item panel, search for **Notebook (2)** and select **Notebook (3)**.

    ![](./Images/01/E1T5S2-2005.png) 

1. In the **New Notebook** pop-up window, leave the name as **default (1)** and click **Create (2)**.

    ![](./Images/e1p2t5p3.png) 

1. After a few seconds, a new notebook with a single cell opens. Each notebook consists of code or markdown cells used for running code or adding formatted text.

     > **Note:** If a pop-up appears for **Note Copilot updates and Git Integration Supporting resources**, click on **Skip for now**.
     >
     > ![](./Images/01/skip-tour.png)

1. Click **Add data items (1)** drop-down under explorer and select **From OneLake catalog (2)** from the drop-down.

    ![](./Images/01/E1T5S5.png)  

1. Select the previously created **Lakehouse_<inject key="DeploymentID" enableCopy="false"/> (1)** then click **Add (2)**.
 
    ![](./Images/img-15.png) 

1. Select the existing cell in the notebook, clear the default code, and replace it with the **variable declaration (1)** below. Then click **&#9655; Run (2)** to execute the cell.

    ```python
   table_name = "sales"
    ```

   ![11](./Images/01/Pg3-Notebook-S2.png) 

    > **Note**: Since this is the first time you've run any Spark code in this session, the Spark pool must be started. This means that the first cell can take few minutes to complete.

    > **Note:** If the Spark pool session remains in the **Starting session**... state for more than 7–8 minutes, it is likely that the workspace Spark metadata has become corrupted, preventing the session from starting successfully. In this scenario, create a **new workspace** and repeat the lab steps starting from Task 2.

1. In the **Ellipsis (...) (1)** menu for the cell (at its top-right) select **Toggle parameter cell (2)**. This configures the cell so that the variables declared in it are treated as parameters when running the notebook from a pipeline.

     ![](./Images/e1p2t5p7.png)

1. Under the parameters cell, use the **+ Code** button to add a new code cell. 

     ![](./Images/E1T5S8-1108.png) 

   >**Note:** You might have to hover your mouse below the output to view the **+ Code** option.

1. Add the following code to it:

    ```python
    from pyspark.sql.functions import *
    
    # Read the new sales data
    df = spark.read.format("csv").option("header","true").option("inferSchema","true").load("Files/new_data/*.csv")

    ## Add month and year columns
    df = df.withColumn("Year", year(col("OrderDate"))).withColumn("Month", month(col("OrderDate")))

    # Derive FirstName and LastName columns
    df = df.withColumn("FirstName", split(col("CustomerName"), " ").getItem(0)).withColumn("LastName", split(col("CustomerName"), " ").getItem(1))

    # Filter and reorder columns
    df = df["SalesOrderNumber", "SalesOrderLineNumber", "OrderDate", "Year", "Month", "FirstName", "LastName", "EmailAddress", "Item", "Quantity", "UnitPrice", "TaxAmount"]

    # Load the data into a managed table
    #Managed tables are tables for which both the schema metadata and the data files are managed by Fabric. The data files for the table are created in the Tables folder.
    df.write.format("delta").mode("append").saveAsTable(table_name)
    ```

     ![](./Images/code1.png) 

    This code loads the data from the sales.csv file that was ingested by the **Copy Data** activity, applies some transformation logic, and saves the transformed data as a **managed table** - appending the data if the table already exists.

1. Verify that your notebooks look similar to the one shown in the image below, and then use the **&#9655; Run all** button on the toolbar to run all of the cells it contains.

    ![Screenshot of a notebook with a parameters cell and code to transform data.](./Images/E1T5S11.png)

1. When the notebook run has completed, click on your  **Lakehouse_<inject key="DeploymentID" enableCopy="false"/> (1)** pane on the left, in the **Ellipsis (...) (2)** menu for **Tables** select **Refresh (3)** and verify that a **sales (4)** table has been created.

    > **Note:** The **sales** table will be visible only after the cell execution is completed.

    ![.](./Images/E1T5S12.png)

1. On the left pane in Notebook, use the ⚙️ **Settings (1)** icon at the top to view the notebook settings. Then, set the **Name** of the notebook to **Load Sales Notebook (2)** and close the settings pane.

     ![.](./Images/E1T5S13-1208.png)

1. In the hub menu bar on the left, click on **fabric-<inject key="DeploymentID" enableCopy="false"/> (1)** and then select your **Lakehouse_<inject key="DeploymentID" enableCopy="false"/> (2)**. 

    ![](./Images/e1p2t5p13.png)

1. In the **Explorer** pane, refresh the **view (1)**. Then expand **Tables (2)**, and select the **sales (3)** table to see a preview of the data it contains.

    ![.](./Images/E1T5S15-1108.png)

    > **Note:** If you table is unable to load and showing as **Unable to load table view**, select the **Table view (1)** option and switch to **File view (2)**. Then open the file displayed.
    >
    >  ![](./Images/unable_to_view.png)

## Task 6: Use SQL to query tables

In this task, you'll use SQL to query tables in a database. You'll write statements to retrieve, filter, and manipulate data, helping you analyze the dataset and build your SQL skills.

1. At the top-right of the Lakehouse page, switch from **Analyze data with (1)** to **SQL analytics endpoint (2)**.

    ![.](./Images/01/E1T6S1-1006.png)

    > **Note**: Please wait a short time until the SQL query endpoint for your lakehouse opens in a visual interface from which you can query its tables

1. Use the **New SQL query** button to open a new query editor, and enter the following SQL query:

    ```SQL
   SELECT Item, SUM(Quantity * UnitPrice) AS Revenue
   FROM sales
   GROUP BY Item
   ORDER BY Revenue DESC;
    ```

    ![](./Images/01/E1T6S2.png)

1. Use the **&#9655; Run** button to run the query and view the results, which should show the total revenue for each product.

    ![](./Images/E2-T5-S3.png)

## Task 7: Create a visual query

In this task, you'll create a visual query in Power BI using Power Query. Start by adding the Sales table to the query editor, select the necessary columns, and applying a Group By transformation to count distinct line items per sales order. Then, review the summarized results.

1. On the toolbar under **New SQL query (1)** drop-down select **New visual query (2)**.

    ![](./Images/p1t7p1.png)

1. In the Lakehouse, navigate to **Schemas**, then to **dbo**, expand the **Tables** folder and select the **sales** table. In the sales table, click on **Ellipsis &#8230; (1)** and select **Insert into canvas (2)**. It is in the new visual query editor pane that opens to create a Power Query. 

    ![](./Images/p1t7p2.png)

1. In the **Manage columns (1)** menu, select **Choose columns (2)**. Then select only the **SalesOrderNumber and SalesOrderLineNumber (3)** columns and click on **OK (4)**.

    ![Account-manager-start](./Images/E1T7S3-1108.png)

    ![Account-manager-start](./Images/E1T7S3.1-1108.png)

1. Click on **Transform table (1)** menu, select **Group by (2)**.

    ![](./Images/E1T7S4.png)

1. Then group the data by using the following **Basic** settings.
   
   - Group by: **SalesOrderNumber (1)**
   - New column name: **LineItems (2)**
   - Operation: **Count distinct values (3)**
   - Column: **SalesOrderLineNumber (4)**
   - Click **OK (5)**

     ![](./Images/p1t7p5.png)

     >**Note:** If you are unable to select **Column**, click **OK** and proceed to the next steps.

1. When you're done, the results pane under the visual query shows the number of line items for each sales order.

    ![Screenshot of a Visual query with results.](./Images/E2-T6-S6.png)

## Task 8: Create a report

In this task, you'll create a report to visualize your data findings. You'll select relevant data, choose appropriate visuals, and organize the report to clearly present insights and support data-driven decisions.

1. First, we will need to create a Semantic model. At the top of the SQL analytics endpoint page, select the **New semantic model** tab.
    
    ![](./Images/e1p2t8p1.png)

2. On the New Smantic Model window, enter the following details and click on **Confirm (4)**:

    - Direct Lake semantic model name: **Lakehouse_<inject key="DeploymentID" enableCopy="false"/> (1)**
    - Storage mode: **Direct Lake on SQL (2)**
    - Select the sales table to be included in the model **(3)**

        ![](./Images/01/E1T8S2-2005.png)

3. Go back to the **fabric-<inject key="DeploymentID" enableCopy="false"/> (1)** workspace from the left pane. Select recently created semantic model named as **Lakehouse_<inject key="DeploymentID" enableCopy="false"/> (2)**

    ![](./Images/e1p2t8p3.png)

4. Click on **Open (1)** to view the table in semantic model.

   ![](./Images/01/E1T8S4.png)

5. Once the model is opened, the **sales** table will appear.

   ![](./Images/E1T8S5.png)

6. Switch to editing mode. In the top menu bar, select the **File (1)** tab. Then select **Create new report (2)**.

    ![](./Images/p1t8p5.png)

7. In the **Data** pane on the right, expand the **sales** table. Then select the following fields:
   
   - **Item (1)**
   - **Quantity (2)**
   - Then, a **Table visualization (3)** is added to the report.

        ![](./Images/p1t8p7.png)

8. Hide the **Data** and **Filters** panes to create more space if required. Then, make sure the **Table visualization is selected (1)** and in the **Visualizations** pane, change the visualization to a **Clustered bar chart (2)** and resize it as shown here.

   ![](./Images/p1t8p8.png)

   ![Screenshot of a report containing a clustered bar chart.](./Images/E2-T7-S5a.png)

9. On the **File (1)** menu, select **Save As (2)**. Then, name the Report as **Item Sales Report (3)** and click **Save (4)** in the workspace you created previously.

   ![](./Images/p1t8p9.png)
   
   ![](./Images/e1p2t8p9.png)

10. In the hub menu bar on the left, select your workspace to verify that it contains the following items:
    - Your lakehouse.
    - The SQL endpoint for your lakehouse.
    - A default dataset for the tables in your lakehouse.
    - The **Item Sales Report** report.

> **Congratulations** on completing the task! Now, it's time to validate it. Here are the steps:
> - If you receive a success message, you can proceed to the next task.
> - If not, carefully read the error message and retry the step, following the instructions in the lab guide. 
> - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

<validation step="09d5de71-ba8a-42fa-a261-b57c69f32b12" />

## 🧾 Summary

In this exercise, you:

- Created a Workspace and a Lakehouse to organize and manage your data environment.
- Imported data into the Lakehouse.
- Explored how a Lakehouse stores data as both files and tables in OneLake.
- Learned that managed tables in the Lakehouse can be queried using SQL.
- Observed that these tables are automatically included in a default dataset, enabling seamless data visualization.

### You have successfully completed the exercise. Click on **Next >>** to proceed with the next exercise.

   ![05](./Images/next-page-1208.png)
