``` SQL
CREATE PROCEDURE [dbo].[JobsWithDynamicCriticalSuccessFactors]
	  @TenantID INT 
AS
	DECLARE @columns NVARCHAR(MAX);
    DECLARE @columnDefinitions NVARCHAR(MAX);
    DECLARE @createTableSql NVARCHAR(MAX);
    DECLARE @insertPivotSql NVARCHAR(MAX);

    -- Step 1: Get all distinct TypeNames and build the column list and column definitions
    SELECT 
        @columns = STRING_AGG(QUOTENAME(TypeName), ','),
        @columnDefinitions = STRING_AGG(QUOTENAME(TypeName) + ' NVARCHAR(MAX)', ', ')
    FROM (SELECT DISTINCT TypeName FROM tsDCriticalSuccessFactor) AS temp;

    -- Step 2: Drop global temp table if it exists
    IF OBJECT_ID('tempdb..##PivotData') IS NOT NULL
        DROP TABLE ##PivotData;

    -- Step 3: Create the global temp table (in dynamic SQL to include dynamic columns)
    SET @createTableSql = '
        CREATE TABLE ##PivotData (
            ServiceDesignID INT PRIMARY KEY, ' + @columnDefinitions + '
        );
    ';
    EXEC sp_executesql @createTableSql;

    -- Step 4: Insert pivoted data into global temp table
    SET @insertPivotSql = '
        INSERT INTO ##PivotData (ServiceDesignID, ' + @columns + ')
        SELECT ServiceDesignID, ' + @columns + '
        FROM (
            SELECT ServiceDesignID, TypeName, InputType
            FROM tsDCriticalSuccessFactor
            WHERE TenantID = ' + CAST(@TenantID  AS nvarchar) + '
        ) AS SourceTable
        PIVOT (
            MAX(InputType) FOR TypeName IN (' + @columns + ')
        ) AS PivotTable;
    ';
    EXEC sp_executesql @insertPivotSql;

    WITH CTE_Main AS (
        SELECT 
             vJob.ID AS JobID
            ,vJob.TenantID
            ,vJob.JobStatusName
            ,vJob.JobStatusColor
            ,vJob.CustomerName
            ,vJob.JobTypeName
            ,vJob.CountryName
            ,vJob.StateProvince
            ,NULL AS Basis -- Basis is not available in North but it will be in South.
            ,vJob.BusinessUnitName
            ,vJob.CategoryName
            ,vJob.EndDate
            ,vJob.SalesContactName AS Personnel
        FROM vJob
    )
    SELECT 
         CTE_Main.JobID
        ,CTE_Main.TenantID
        ,CTE_Main.JobStatusName
        ,CTE_Main.JobStatusColor
        ,CTE_Main.CustomerName
        ,CTE_Main.JobTypeName
        ,CTE_Main.CountryName
        ,CTE_Main.StateProvince
        ,CTE_Main.Basis 
        ,CTE_Main.BusinessUnitName
        ,CTE_Main.CategoryName
        ,CTE_Main.EndDate
        ,CTE_Main.Personnel
        ,PivotData.*
    FROM CTE_Main
    JOIN tServiceDesign ON tServiceDesign.JobID = CTE_Main.JobID
    LEFT JOIN ##PivotData PivotData ON PivotData.ServiceDesignID = tServiceDesign.ID
    WHERE CTE_Main.TenantID = @TenantID;

    --select * from ##PivotData; 

    DROP TABLE ##PivotData;
RETURN 0
```

