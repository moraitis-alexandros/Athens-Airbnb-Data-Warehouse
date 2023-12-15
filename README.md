# Athens-Airbnb-Data-Warehouse
Engineered a data warehouse in SQL Server using Airbnb data for the city of Athens, constructing a robust pipeline to extract, clean, and transform data with a star schema for efficient querying. Presented insights through Power BI visualization.

**Objective:**
This project aims to demonstrate the end-to-end process of transforming raw Airbnb data into a structured data warehouse and creating insightful visualizations using Power BI.

**Tools Used:**
- SQL Server
- MS Visual Studio (ETL Pipeline)
- Power BI

## Project Overview:
1. **Data Acquisition:** 
   Obtained raw Airbnb data from [http://insideairbnb.com/] website. We focused only in the city of Athens and we downloaded three files in csv that included reservations from 27-03-23 until 23-03-24 as  above:
   
    **Calendar data** file that stores daily reservations on airbnb platform (4.154.445 records)

    **Listings data** file that stores information about the appartments available for rent on airbnb platform (11.382 records)

    **Neighbourhoods geojson** file that stores topographic information about Athens neightbourhoods (longtitude, latitude exc)

2. **Data Transformation in SQL Server:** By applying the literature (Kimball) concerning the steps for creating a Data Warehouse (DW Design Steps), on the dataset the following emerged:

      **Step 1: Choose the process to model:** Reservations for properties on the Airbnb platform will be modeled for the period from 27-03-23 to 27-03-24.

      **Step 2: Choose the granularity of the process:** Reservations per day, per property, per host, per region, and combinations thereof.

      **Step 3: Choose the dimensions:** Date, Listing (property), host (person who owns the property), neighborhood (property's neighborhood).

      **Step 4: Choose the measures:** Listing count, Host count, Average Listing Price, and some calculated metrics that will be presented in more detail later in the work, specifically during the                 analysis of the cube.

Based on the above steps, in creating the database on the SQL server, we arrived at the following:A database named ProjectDW, four dimension tables as follows: Date Dimension named date_dim, Listing Dimension named listing dim, Host Dimension named host dim, Neighborhood Dimension named neighborhood dim. Finally we came up with a fact table named listings booking fact.

   **Date Dimension**
    We run the code that generate the table for dates and loads date data through a looping process.

    ```sql
-- CREATE TABLE [dbo].[DimDate](
       [DateKey] [int] NOT NULL,
       [FullDateAlternateKey] [date] NOT NULL,
       [DateString] [varchar](10) NULL, /*date evala*/
       [DayOfWeek] [tinyint] NOT NULL,
       [DayOfWeekName] [nvarchar](10) NOT NULL,
       [DayOfMonth] [tinyint] NOT NULL,
       [DayOfYear] [smallint] NOT NULL,
       [WeekOfYear] [tinyint] NOT NULL,
       [MonthName] [nvarchar](10) NOT NULL,
       [MonthOfYear] [tinyint] NOT NULL,
       [CalendarQuarter] [tinyint] NOT NULL,
       [CalendarYear] [smallint] NOT NULL,
       [IsWeekend] [bit] NOT NULL,
       [IsLeapYear] [bit] NOT NULL,
 CONSTRAINT [PK_DimDate] PRIMARY KEY CLUSTERED
(
       [DateKey] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]
GO
SET ANSI_PADDING OFF
GO
ALTER TABLE [dbo].[DimDate] ADD  CONSTRAINT [DF_DimDate_IsWeekend]  DEFAULT ((0)) FOR [IsWeekend]
GO
ALTER TABLE [dbo].[DimDate] ADD  CONSTRAINT [DF_DimDate_IsLeapYear]  DEFAULT ((0)) FOR [IsLeapYear]
GO


 CREATE TABLE [dbo].[DimTime](
       [TimeKey] [int] NOT NULL,
       [TimeAltKey] [time](0) NULL,
       [HourOfDay] [tinyint] NULL,
       [MinuteOfHour] [tinyint] NULL,
       [SecondOfMinute] [tinyint] NULL,
       [TimeString] [varchar](8) NULL,
 CONSTRAINT [PK_DimTime] PRIMARY KEY CLUSTERED
(
       [TimeKey] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON) ON [PRIMARY]
) ON [PRIMARY]

```
  
   *We used only dates within the loop from 1-1-2007 to 31-12-2025. The reason for going back so far, despite the fact table covering the years 2023 and 2024, is that we identified the attribute       "host since" (indicating when the host became a member of Airbnb) in the host dimension with a minimum value of 2008. Therefore, in a potential connection between the host dimension and the date    dimension, the relevant date should exist.*


Time Dimension
Flag 
Feature Creation









4. **ETL Pipeline Creation in MS Visual Studio:**
   - Developed an ETL pipeline to extract, transform, and load data into a star schema data warehouse.
   - Implemented a star schema for optimized querying and analysis.

5. **Visualization in Power BI:**
   - Imported the transformed data into Power BI.
   - Created interactive visualizations and insightful


