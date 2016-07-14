GasBuddy SQL Standards
======================

Naming
======
SQL views, indices, and stored procedures all begin with a prefix which indicates their type.
Index names contain the name of the table followed by the columns in the index as well as other important information (e.g. "filtered", "covering", etc.). 
Stored procedure names contain the primary table name, followed by the operation, followed by the version number.

```
views: 		[dbo].[vw_station_with_prices]
indexes: 	[dbo].[idx_members_id]
stored procedure with an individual object as output: 	[dbo].[usp_members_get_by_id]
stored procedure with a list of objects as output: 	[dbo].[usp_member_comments_list_by_member_id]
versioning:	[dbo].[usp_member_comments_list_by_member_id_v2]
```

SQL tables and view are **plural, lower case and underscore separated**. 

```
[dbo].[awards]
[dbo].[members]
[dbo].[member_comments]
```

Formatting
==========
All T-SQL reserved words are **uppercase**. All identifiers should be delimited by **square brackets**.

```
CREATE TABLE [dbo].[price_hist2](
	[id] [BIGINT] IDENTITY(1,1) NOT NULL,
	[ph2_event_id] [BIGINT] NOT NULL,
	[ph_id] [BIGINT] NOT NULL,
	[sm_id] [INT] NOT NULL,
	[fuel_product_id] [SMALLINT] NOT NULL,
	[price_type_id] [TINYINT] NOT NULL,
	[price] [DECIMAL](5, 2) NOT NULL,
	[source] [SMALLINT] NOT NULL,
	[station_tme] [DATETIME] NOT NULL,
	[server_tme] [DATETIME] NOT NULL,
	[member_id] [VARCHAR](30) NOT NULL,
	[bad_price] [BIT] NOT NULL
);
```

For stored procedures: Add a comment header (as below) to the script.
All SQL statements should end with semicolon(;).
All the references to identifiers should include the schema name.
Do not add database for local objects.
Respect CASE used in identifiers and columns definition.
Use a new line per column in the select.
Commas are placed at the end of the column name.
Use SCOPE_IDENTITY() to get the last identity value.


```
-- =============================================
-- Author:      [Name]
-- Create date: [mm/dd/yyyy]
-- Description:	[add description]
-- Update Log:
-- VERSION  DATE        AUTHOR     DESCRIPTION
-- V#       yyyy-mm-dd	<name>     <What changed?>
-- =============================================
CREATE PROCEDURE [dbo].[usp_fuel_products_get_by_id] 
  @id INT
AS
BEGIN
  SET NOCOUNT ON;

  SELECT id,
    display_name,
    fuel_group_id,
    octane_id,
    ethanol_id,
    category_id
  FROM
    [dbo].[fuel_products] WITH(NOLOCK)
  WHERE
    id = @id;
END
```

When joining, use meaningful aliases for the tables and identify all columns with their corresponding alias.
Do not use "SELECT *".

```
  SELECT srdt.sm_id, 
    srdt.rating_value,
    SUM(srdt.total_ratings) AS 'total_ratings'
  FROM
    [dbo].[station_rating_day_totals] srdt WITH (NOLOCK)
    INNER JOIN @sm_ids smids ON smids.id = srdt.sm_id
  WHERE
    srdt.day BETWEEN @min_date AND @max_date
    AND srdt.rating_category_id = @rating_category_id
  GROUP BY
    srdt.sm_id, srdt.rating_value
  ORDER BY
    srdt.sm_id;
```

Source Control
==============
Before making a pull request for your change, validate that your scripts are not causing errors/warnings in the solution.
At some point we will put CI infrastructure around this, but for now we all need to do it manually.
If files were added or deleted make sure to save the project.

File format
---------------
Do not include USE or SET statementes outside the Stored Procedure.
For external references, replace the database name with the corresponding variable.

Common Warnings
---------------
Warning	459		SQL71562: Procedure: [dbo].[usp_get_my_station_photos_pages] contains an unresolved reference to an object. Either the object does not exist or the reference is ambiguous because it could refer to any of the following objects: [Gasbuddy].[dbo].[station_master_photo].[sp]::[stationId] or [Gasbuddy].[dbo].[station_master_photo].[stationId].	C:\GasBuddyDBs\gb-sql-server-db\GBPhotos\dbo\Stored Procedures\usp_get_my_station_photos_pages.sql	46	18	GBPhotos

```
	select      sp.stationId, 
				count(*) as total
	from		Gasbuddy..station_master_photo as sp with(nolock)
	where		sp.unique_member_id = 3849
	group by	sp.stationId
	order by	sp.stationId desc;
```

Fix: For external references we need to change the name of the database for the corresponding DB variable
			
```
	SELECT      sp.stationId, 
				count(*) AS total
	FROM		[$(Gasbuddy)].[dbo].[station_master_photo] as sp with(nolock)
	WHERE		sp.unique_member_id = 3849
	GROUP BY	sp.stationId
	ORDER BY	sp.stationId desc;
```		

Warning	891		SQL71558: The object reference [dbo].[MSA].[id] differs only by case from the object definition [dbo].[MSA].[ID].	C:\GasBuddyDBs\gb-sql-server-db\GEO\dbo\Stored Procedures\usp_gasbuddy_get_competitors_by_area_and_smid.sql	6	1	GEO

```
	SELECT		@areaGeog = geog
	FROM		[dbo].[MSA] with (nolock)
	WHERE		id = @areaID;
```

Fix: Since ID was defined with upper case in MSA table:
			

```
	SELECT		@areaGeog = geog
	FROM		[dbo].[MSA] with (nolock)
	WHERE		ID = @areaID;
```		
