#  Exercise 3: Add GPU-accelerated Fabric Data Warehouse context

### Estimated Duration: **30 Minutes**

## Overview

In this exercise, you will explore the GPU-accelerated query engine for Fabric Data Warehouse and analyze warehouse query performance hands-on. Announced at Microsoft Build 2026 and built on NVIDIA accelerated computing, query acceleration lets the warehouse query optimizer automatically route eligible SQL operations such as large joins, aggregations, and scans to GPU execution, while ineligible queries fall back seamlessly to the standard CPU engine with no query rewrites required. Microsoft's benchmarks show the largest gains at high concurrency, where accelerated response times remain nearly flat as user load increases. The feature is controlled by a single **workspace-level** setting that applies to all Data Warehouses and SQL analytics endpoints in the workspace.

You will review the workspace-level Data Warehouse settings where the query acceleration capability surfaces, run a multi-table analytical query against the **Data Warehouse-<inject key="DeploymentID" enableCopy="false"/>** you built in Exercise 1, and use the built-in **queryinsights** views to measure and compare query performance the same measurement approach you would use to evaluate the GPU-accelerated path against the CPU path on a production workload.

> **Note:** This exercise uses the same **fabric-<inject key="DeploymentID" enableCopy="false"/>** workspace and **Data Warehouse-<inject key="DeploymentID" enableCopy="false"/>** that you created in **Exercise 1 of this lab**. Sign in and navigate to that workspace before beginning Task 1.

## Lab objectives

You will be able to complete the following tasks:

- Task 1: Review the GPU query acceleration capability and workspace Data Warehouse settings
- Task 2: Run an analytical query and measure performance with query insights

## Task 1: Review the GPU query acceleration capability and workspace Data Warehouse settings

In this task, you will sign in to Microsoft Fabric and review the workspace-level Data Warehouse settings the surface where GPU query acceleration is managed. Query acceleration is currently an opt-in early access preview that Microsoft is rolling out tenant by tenant, so you will confirm what your environment exposes today and learn where the capability appears once a tenant is onboarded.

1. Open a browser and go to <https://app.fabric.microsoft.com>. Sign in by using the following credentials:

    - **Email/Username:** <inject key="AzureAdUserEmail" enableCopy="true"/>
    - **Password:** <inject key="AzureAdUserPassword" enableCopy="true"/>

1. From the left pane, select the **fabric-<inject key="DeploymentID" enableCopy="false"/>** workspace.

    ![](./Images/.png)

1. In the **fabric-<inject key="DeploymentID" enableCopy="false"/>** workspace, select **Workspace settings** from the toolbar.

    ![](./Images/IQ149.png)

1. In the workspace settings pane, select **Data Warehouse (1)** and review the available settings **(2)**.

    ![](./Images/CU-ws-dw-settings.png)

1. Observe the settings listed in the **Data Warehouse** section, such as **Collations** and **New metadata sync** the exact list varies as Fabric evolves. These standard settings govern string handling and SQL endpoint synchronization; none of them relates to GPU. This section is, however, where **Query acceleration (Preview)** appears as an additional toggle on tenants that have been onboarded to the GPU early access program.

    > **Note:** GPU query acceleration is an opt-in early access preview that Microsoft is rolling out gradually in most tenants, including this lab environment, the toggle is not yet visible, and that is expected. When a tenant is onboarded, enabling the single workspace-level toggle routes eligible queries from every Data Warehouse and SQL analytics endpoint in the workspace to GPU execution automatically, with no query rewrites. Everything in the remainder of this exercise runs identically on the standard CPU engine.

1. Close the workspace settings pane.

    > **Note:** Because query acceleration is a workspace-level setting, no per-warehouse configuration is required. When enabled, the warehouse query optimizer decides per query fragment whether GPU execution applies ineligible operations automatically run on the CPU engine and return identical results.

## Task 2: Run an analytical query and measure performance with query insights

In this task, you will scale the fact table to roughly two million rows to create a workload where performance measurement is meaningful, run a multi-table join query that exercises all four warehouse tables, and use the warehouse's built-in **queryinsights** views to measure execution time the same measurement technique used to compare GPU-accelerated and CPU query performance on production workloads. The scaled workload you build here also serves as a ready-made benchmark: once a tenant is onboarded to GPU query acceleration, re-running this exact workload with the workspace toggle enabled quantifies the GPU gains.

1. Return to the **fabric-<inject key="DeploymentID" enableCopy="false"/> (1)** workspace and open **Data Warehouse-<inject key="DeploymentID" enableCopy="false"/> (2)**.

    ![](./Images/IQ150.png)

1. Open a **New SQL query** from the toolbar, enter the following query, and click **&#9655; Run** to build a scaled fact table of approximately **two million rows** by multiplying the existing sales orders. This gives the warehouse a workload large enough for meaningful performance measurement.

    ```SQL
    CREATE TABLE dbo.FactSalesOrderLarge AS
    SELECT
        so.SalesOrderDateKey,
        so.ProductKey,
        so.CustomerKey,
        so.Quantity,
        so.SalesTotal
    FROM dbo.FactSalesOrder AS so
    CROSS JOIN (
        SELECT TOP 500 ROW_NUMBER() OVER (ORDER BY (SELECT NULL)) AS n
        FROM sys.objects AS a CROSS JOIN sys.objects AS b
    ) AS multiplier;
    ```

    > **Note:** The statement may take a minute or two to complete. When it finishes, run `SELECT COUNT(*) FROM dbo.FactSalesOrderLarge;` in the same query window if you want to confirm the row count.

1. Open a **New SQL query (1)** from the toolbar, enter the following **query (2)**, and click **&#9655; Run (3)**. Note the total run time displayed in the results pane at the bottom of the screen.

    ```SQL
    SELECT
        p.Category,
        c.CountryRegion AS SalesRegion,
        d.[Year] AS SalesYear,
        d.MonthName,
        SUM(so.SalesTotal) AS TotalRevenue,
        SUM(so.Quantity) AS TotalUnits
    FROM FactSalesOrderLarge AS so
    JOIN DimProduct AS p ON so.ProductKey = p.ProductKey
    JOIN DimCustomer AS c ON so.CustomerKey = c.CustomerKey
    JOIN DimDate AS d ON so.SalesOrderDateKey = d.DateKey
    GROUP BY p.Category, c.CountryRegion, d.[Year], d.MonthName
    ORDER BY SalesYear, p.Category;
    ```

    ![](./Images/IQ153.png)

1. Run the same query a **second time** and note the total run time again. The second execution typically completes faster because of warehouse caching this illustrates why single-run timings are unreliable and structured measurement matters.

1. Open a **New SQL query (1)**, enter the following **query (2)** against the built-in **queryinsights** views, and click **&#9655; Run (3)** to retrieve the execution history of your analytical query:

    ```SQL
    SELECT TOP 10
        distributed_statement_id,
        submit_time,
        total_elapsed_time_ms,
        allocated_cpu_time_ms,
        data_scanned_disk_mb,
        data_scanned_memory_mb,
        result_cache_hit
    FROM queryinsights.exec_requests_history
    WHERE command LIKE '%FactSalesOrderLarge%'
    ORDER BY submit_time DESC;
    ```

    ![](./Images/IQ155.png)

    > **Note:** Query insights data is populated asynchronously and can take up to 15 minutes to appear. If your query returns no rows, wait a few minutes and run it again you can proceed to review the column descriptions in the next step while you wait.

1. Review the output and compare your two runs of the analytical query:

    - **total_elapsed_time_ms (1)**: end-to-end duration of each execution compare the first and second runs.
    - **data_scanned_disk_mb** and **data_scanned_memory_mb (2)**: how much data was read from disk versus memory the second run typically shifts toward memory.
    - **result_cache_hit (3)**: whether the result was served from cache.

    ![](./Images/IQ156.png)

1. Record the durations from the query insights output for comparison:

    | Run | total_elapsed_time_ms |
    |---|---|
    | First execution | _______ ms |
    | Second execution | _______ ms |

    > **Note:** GPU query acceleration delivers its largest gains on big scans, joins, and aggregations and above all at high concurrency, where many users or agents query the warehouse simultaneously and accelerated response times remain nearly flat. The workload you just measured a multi-million-row join measured through **queryinsights.exec_requests_history** is exactly the controlled benchmark you would re-run after enabling the **Query acceleration (Preview)** workspace setting to quantify the GPU-accelerated path against the CPU baseline you recorded today.

## Summary

In this exercise, you:

- Reviewed the workspace-level Data Warehouse settings and learned where the **Query acceleration (Preview)** toggle for GPU-accelerated Fabric Data Warehouse appears once a tenant is onboarded to the early access program.
- Scaled the fact table to approximately two million rows, ran a multi-table join query across all four warehouse tables, and measured execution performance using the built-in **queryinsights.exec_requests_history** view comparing elapsed time, data scanned, and cache behavior across runs, and establishing a CPU baseline benchmark that can be re-run to quantify GPU acceleration once the tenant is onboarded.

### You have successfully completed the lab.