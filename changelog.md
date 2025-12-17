# Changelog - Analyzing Databel's Customer Churn in Excel

This changelog documents the steps taken in the Excel-based data analysis project.

## Version 1.0 (2025-02-13) - Initial Analysis

- **Data Source:**
  - File: `databel-customer-data.xlsx`
  - Location: [/excel-databel-customer-churn/databel-customer-churn-data.xlsx](/excel-databel-customer-churn/databel-customer-churn-data.xlsx)
  - Description: Customer, contract, subscription and usage information
  - Metadata: [DataCamp metadata sheet](/excel-databel-customer-churn/databel-customer-churn-metadata-sheet.pdf 'Metadata sheet for customer churn data')

- **Data Cleaning and Preprocessing:**
    - Step 1: Create copy of workbook with name `databel-customer-churn-dashboard.xlsx`
    - Step 2: Change worksheet name from "Databel - Aggregate" to "Aggregate"
    - Step 3: On the "Aggregate" worksheet: Insert table named "Aggregate"
    - Step 4: Change worksheet name from "Databel - Customer" to "Customer"
    - Step 5: On the "Customer" worksheet: Insert table named "Customer"
    - Step 6: On the "Customer" worksheet: Remove duplicates on "Customer ID" = No duplicate values found. *Rationale: Ensure data accuracy.*
    - Step 7: On the "Customer" worksheet: Replace blank values in "Churn Category" with "Unknown" (4918 replacements). *Rationale: Standardize missing churn category values.*
    - Step 8: On the "Customer" worksheet: Replace blank values in "Churn Reason" with "Unknown" (4918 replacements). *Rationale: Standardize missing churn category values.*
    - Step 9: On the "Customer" worksheet: Changed "Churn Category" to "Unknown" where "Churn Reason" is "Don't know" (123 replacements). *Rationale: To standardize the representation of unknown churn reasons across categories.*
    - Step 10: On the "Customer" worksheet: Add new column "Churned" with formula `=IF([@[Churn Label]]="Yes",1,IF([@[Churn Label]]="No",0,""))`. *Rationale: Convert "Churn Label" to a binomial value.*
    - Step 11: On the "Aggregate" worksheet: Add new column "Age Demographics" with formula `=IF([@Senior]="Yes","Senior",IF([@[Under 30]]="Yes","Under 30","Other"))`. *Rationale: Segment customers by age demographics to identify trends and patterns within specific customer segments.*
    - Step 12: On the "Aggregate" worksheet: Add new column "Age Group" with formula `=IFS([@Age]<=28,"19-28",[@Age]<=38,"29-38",[@Age]<=48,"39-48",[@Age]<=58,"49-58",[@Age]<=68,"59-68",[@Age]<=78,"69-78",[@Age]<=88,"79-88",TRUE,"Other")`. *Rationale: Segment customers by age groups to identify trends and patterns within specific customer segments.*
    - Step 13: On the "Aggregate" worksheet: Add new column "Data Usage" with formula `=IFS([@[Avg Monthly GB Download]]>10,"10 or more GB",[@[Avg Monthly GB Download]]<5,"Less than 5 GB",AND([@[Avg Monthly GB Download]]>=5,[@[Avg Monthly GB Download]]<=10),"Between 5 and 10 GB")`. *Rationale: Segment customers based on their average monthly gigabytes downloaded to analyze how data usage levels correlate with other customer attributes.*

- **Data Analysis:**
  - Step 1: Created a new "Analysis" worksheet to calculate key metrics and KPIs.
  - Step 2: Calculated the following metrics on the "Analysis" worksheet:
    - Total Customers (using `COUNTA` function)
    - Churned Customers (using `SUM` function)
    - Churn Rate (calculated as Churned Customers / Total Customers)
  - Step 3: Created a pivot table on the "Analysis" worksheet using the "Customer" table data to analyze churn reason by category. *Rationale: To identify the leading categories contributing to customer churn.*
  - Step 4: Created a pie chart visualizing churn reason by category from the pivot table and placed it on a new "Dashboard" worksheet.
  - Step 5: Created a dynamic, interactive churn analysis section on the "Analysis" worksheet. This section allows users to filter churn reasons by category.
    - Added a form control combo box linked to cell B16, listing the churn categories from the pivot table.
    - Created a dynamic chart title in cell B17, updating based on the combo box selection (e.g., "Price Churn Analysis").
    - Implemented a dynamic filtering mechanism in cell A20 using the `LET`, `SWITCH`, `FILTER` and `PIVOTBY` functions. This formula filters the 'Churn Reason' and 'Churned' columns based on the selected category in the combo box and generates a new pivot table.
    - Created dynamic named ranges referencing the output of the filtering formula in A20.
    - Inserted a donut chart visualizing the filtered churn reasons, with its title linked to the dynamic title in cell B17.  This chart updates dynamically as the combo box selection changes.
    *Rationale: To enable interactive exploration of churn reasons by category, allowing users to drill down into specific areas of interest and gain deeper insights into the drivers of churn.*

## Version 1.1 (2025-02-15) - Continued Analysis

  - Step 6: Grouped the churn reason donut chart and its associated combo box and moved them to the "Dashboard" worksheet.
  - Step 7: On the "Analysis" worksheet, created a pivot table using the "Aggregate" table data to analyze the number of customers and churn rate per age group.
  - Step 8: Created a combination chart on the "Analysis" worksheet, visualizing the number of customers per age group as a column chart and the corresponding churn rate as a line graph. *Rationale: To visualize and analyze customer distribution and churn rate trends across different age groups, providing insights into age-related churn patterns.*
  - Step 9: Moved the age group analysis combination chart to the "Dashboard" worksheet.
  - Step 10: On the "Analysis" worksheet, extracted a list of unique state codes from the "Customer" table using the `UNIQUE` and `SORT` functions, ensuring the list is sorted alphabetically.
  - Step 11: On the "Analysis" worksheet, calculated the following metrics for each state:
    - Total Customers (using `SUMIFS`)
    - Churned Customers (using `SUMIFS`)
    - Churn Rate (calculated as Churned Customers / Total Customers, using `IFERROR` to handle potential division by zero errors)
  - Step 12: Created a map visualization on the "Dashboard" worksheet using the calculated state-level churn rate data. *Rationale: To visualize geographic patterns in churn rates and identify states with high or low churn, enabling targeted interventions and resource allocation.*

## Version 1.2 (2025-02-17) - Continued Analysis

  - Step 13: On the "Aggregate" worksheet, created a new "Account Age" column using the following formula to categorize account length: `=IFS([@[Account Length (in months)]]<=12,"months 1 to 12",[@[Account Length (in months)]]<=24,"years 1 to 2",[@[Account Length (in months)]]<=36,"years 2 to 3",[@[Account Length (in months)]]<=48,"years 3 to 4",[@[Account Length (in months)]]<=77,"years 4 or more")`. *Rationale: To identify churn patterns across different stages of the customer lifecycle.*
  - Step 14: On the "Analysis" worksheet:
    - Extracted a list of unique "Account Age" categories, sorted alphabetically.
    - Calculated the churn rate for each combination of "Age Demographic" and "Account Age" using `SUMIFS` to sum total customers and churned customers, and then dividing churned customers by total customers.
    - Added a "Total" column to the table, representing the total churn for each "Account Age" category.
  - Step 15: On the "Analysis" worksheet:
    - Applied conditional formatting to the churn rate table to create a heatmap visualization of churn by "Age Demographic" and "Account Age."
    - Used data bars conditional formatting in the "Total" column to create a bar graph visualizing the percentage of total churn for each "Account Age" category.
    - Added the title "Churn Rate by Account Age and Demographics" to the table.
  - Step 16: On the "Dashboard" worksheet:
    - Copied the formatted churn rate table from the "Analysis" worksheet and inserted it as a linked picture within a shape.
    - Inserted a text box linking to the table title in the "Analysis" worksheet and group with the linked picture. *Rationale: To visualize and analyze churn rate patterns across different account age ranges and demographic groups, providing insights into factors influencing churn and enabling targeted interventions.*
  - Step 17: Enhanced the "Dashboard" worksheet's visual presentation:
    - Added the "Databel" logo.
    - Applied formatting adjustments to existing shapes to improve clarity and visual appeal. *Rationale: To enhance the professional presentation of the dashboard and reinforce branding.*

## Version 1.3 (2025-02-18) - Continued Analysis

  - Step 18: Enhanced the state-level churn analysis, mirroring the interactive filtering approach used for churn categories and reasons (similar to Step 5). On the "Analysis" worksheet:
    - Inserted a combo box to filter the state data by metric (Total Customers, Churned Customers, or Churn Rate).
    - Implemented a dynamic chart title, allowing the map visualization to update based on the selected metric.
    - Created named ranges for all columns in the state information table.
    - Created a helper table.  The first column extracts the state information. The second column uses a `CHOOSE` function to select the appropriate metric data based on the combo box input. The third column uses `IF` and `TEXT` functions to apply conditional formatting to the selected metric data.
    - Updated the data reference in the map visualization to point to the new helper table.
    - Moved the combo box to the "Dashboard" worksheet.
    *Rationale: To enable interactive exploration of state-level churn data by different metrics, providing users with the ability to analyze total customers, churned customers, and churn rate on the map visualization.*
  - Step 19: Added key metric summaries to the "Dashboard" worksheet by inserting shapes containing references to the "Total Customers", "Churned Customers", and "Churn Rate" metrics (calculated in Step 2).
  - Step 20: Performed significant cosmetic enhancements to the "Dashboard" worksheet to improve its overall appearance and user experience. *Rationale: To provide at-a-glance summaries of key churn metrics on the dashboard and to enhance the dashboard's visual appeal and usability.*

## Version 1.4 (2025-02-20) - Fixes

- **Fixes:**
  - **Map Visualization Color Scaling:** Resolved an issue where the color scaling for the map visualization was inconsistent and reversed, causing incorrect representation of Total Customers, Churned Customers, and Churn Rate.  The issue stemmed from formatting applied in the helper table to display percentage values correctly, which interfered with the visualization tool's interpretation of the data. This has been corrected by removing formatting from the helper table and using the underlying numeric data for visualization.
  - **Custom Gradient Legend:** Implemented a custom gradient legend for the map visualization to provide a clear and accurate representation of the data ranges.  This replaces the default legend, which displays percentage values as 0 or 1. The custom legend uses a gradient rectangle with labels indicated "Low" or "High".

- **Details:**
  - The column in the helper table to format the data was removed.
  - The map visualization reference that pointed to the formatted column was updated to point to the unformatted data column instead.
  - A custom legend was added using drawing tools and grouped with the map visualization.

- **Impact:**
  - The map visualization now accurately reflects the data, with correct color scaling for all metrics.
  - The custom legend provides a clear and user-friendly interpretation of the churn rate range.

- **Testing:**
  - Verified that the map visualization displays correct colors for all data points.
  - Confirmed that the custom legend accurately reflects the scale with low values a light green and high values a dark green.
  - Tested filtering functionality to ensure it does not impact color scaling.

## Version 1.5 (2025-02-22) - Fixes

- **Enhancements:**
  - **Churn Reason Visualization Update:**
    - Changed the "Churn Reason Categories" pie chart visualization to a bar chart for improved readability.
    - Sorted the bar chart in descending order to highlight the most frequent churn reasons.
    - Applied a darker color to the bar representing the largest value, further emphasizing the primary churn driver.
  - **Dynamic, interactive churn analysis section:**
    - On the "Analysis" worksheet, modify the churn reason filter (see Step 5 above) to calculate the churn rate for each reason as opposed to churned number.
    - Updated cell formatting to display the calculated churn rates as percentages, enhancing data clarity.
  - **Dynamic Reason Churn Analysis Chart Update:**
    - Verified that the values on the chart are now displayed as percentages to be consistent with "Churn Reason Categories" visualization.
    - Changed the "*Reason Category* Churn Analysis" donut chart to a bar chart for improved data representation and easier comparison of churn reasons.
    - Verified that the reason filter displays the reasons for each category correctly.
