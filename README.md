# Urban-Infrastructure-Service-Intelligence
A Data-Driven Analysis of Municipal Maintenance Requests, Service Resolution, and Geographic Hotspots
Project type: Industry-oriented Business Intelligence / Data Analytics
Domain: Smart Cities, Public Infrastructure, Municipal Operations
Analysis period: 1 January 2025 – 31 December 2025
Backlog ageing reference date: 1 January 2026

Table of Contents
1. Project Overview
2. Business Problem
3. Aim and Objectives
4. Key Business Questions
5. Dataset and Scope
6. Technology Stack
7. Methodology
8. Data Preparation and Feature Engineering
9. Data Quality and Validation
10. SQL Analysis and Key Results
11. Power BI Dashboard
12. Key Findings
13. Business Recommendations
14. Limitations
15. Repository Structure
16. How to Reproduce the Analysis
17. Conclusion and Future Scope
18. Data Source and References
1. Project Overview
Urban Infrastructure Service Intelligence is an end-to-end Business Intelligence project that analyses New York City 311 service requests related to public infrastructure. The project explores the volume and distribution of municipal maintenance requests, monthly demand patterns, resolution performance, unresolved work, geographic concentrations, and data-quality issues.

The analysis focuses on four infrastructure-related complaint categories:
- Street Condition
- Street Light Condition
- Sewer
- Sidewalk Condition
The project uses Python for data preparation, SQL and SQLite for structured analysis, and Power BI Service for interactive reporting. The final solution is organized into a five-page dashboard designed to support operational monitoring and evidence-based discussion.

Dataset size: 156,034 records.
2. Business Problem
Municipal operations teams receive service requests across multiple infrastructure categories and geographic areas. Without a consolidated analytical view, it can be difficult to understand which issues generate the greatest demand, where requests are concentrated, whether request volumes change over time, and how much unresolved work is ageing.

Inconsistent status and closure-date information can also affect the reliability of performance reporting.

This project addresses the need for a structured business intelligence solution that converts service-request records into useful operational indicators and visual insights.
3. Aim and Objectives
Aim
To build a data-driven Business Intelligence solution that analyses municipal infrastructure service requests, identifies demand and geographic patterns, evaluates recorded resolution performance, and quantifies backlog ageing and data-quality issues.
Objectives
- Measure total request volume and compare the four selected complaint categories.
- Identify monthly demand patterns and month-over-month changes.
- Compare request concentrations across boroughs and ZIP codes.
- Calculate status-based resolution rates and valid resolution-time statistics.
- Quantify records without closure dates and classify their age.
- Validate unique identifiers, missing closure dates, status-date mismatches, and negative resolution durations.
- Develop an interactive five-page Power BI dashboard.
- Translate the findings into practical operational recommendations.
4. Key Business Questions
1. Which infrastructure complaint categories generate the highest request volumes?
2. How does service-request volume change month by month?
3. Which boroughs and ZIP codes account for the largest recorded volumes?
4. How do resolution rates and resolution times vary by complaint category?
5. How large is the backlog, and how old are records without closure dates?
6. What data-quality issues should managers consider before interpreting the results?
5. Dataset and Scope
Data source
The project uses the official NYC Open Data dataset:

311 Service Requests from 2020 to Present
- Dataset page: https://data.cityofnewyork.us/Social-Services/311-Service-Requests-from-2020-to-Present/erm2-nwe9
- API endpoint: https://data.cityofnewyork.us/resource/erm2-nwe9.json
Scope
Dimension	Description
Geography	New York City
Reporting period	1 January 2025 – 31 December 2025
Categories	Street Condition, Street Light Condition, Sewer, Sidewalk Condition
Records	156,034
Original source fields	12
Backlog reference date	1 January 2026
Main output	Five-page Power BI dashboard


Important scope note
This is a historical analysis of 2025 records. Backlog ageing is calculated against the fixed reference date of 1 January 2026. The backlog figures must not be interpreted as a live or current operational backlog.
6. Technology Stack
Technology	Purpose
Python	Data extraction support, cleaning, transformation, feature engineering, and exploratory analysis
Pandas	Data filtering, date conversion, validation, aggregation, and CSV export
SQL	Repeatable analysis of categories, trends, resolution, backlog, agencies, and geographic hotspots
SQLite	Lightweight relational database for structured storage and querying
Power BI Service	Interactive dashboard, KPI cards, charts, and report presentation
Excel / CSV	Data inspection, intermediate exports, and exchange between tools
GitHub	Version control, project documentation, and portfolio presentation


7. Methodology
The project follows a descriptive Business Intelligence methodology.
End-to-end workflow
1. Data acquisition: Access NYC Open Data and select 2025 records for the four chosen complaint categories.
2. Data inspection: Review the schema, row count, field types, missing values, and category values.
3. Data cleaning: Standardize date fields, inspect missingness and duplicates, and prepare the analytical dataset.
4. Feature engineering: Calculate resolution duration, backlog age, and data-validation flags.
5. Database loading: Store the cleaned data in SQLite.
6. SQL analysis: Run structured queries for request volumes, trends, resolution, backlog, agency performance, and geographic concentrations.
7. Dashboard development: Build five report pages in Power BI Service.
8. Validation: Reconcile key Power BI values against SQL results and document limitations.
Analytical methods
- Descriptive statistics and frequency analysis
- Percentage and share-of-total analysis
- Monthly time-series aggregation
- Month-over-month percentage change
- Category and agency comparison
- Average and median resolution-time analysis
- Backlog ageing segmentation
- Borough and ZIP-code ranking
- Data-quality validation
8. Data Preparation and Feature Engineering
The selected records were filtered to the 2025 calendar year and four complaint categories. Date fields were converted to date-time values to support monthly aggregation and elapsed-time calculations.

The prepared master dataset is named:

urban_infrastructure_311_cleaned.csv

The SQLite database is named:

urban_infrastructure.db

The analysis table is:

service_requests
Important fields
Field	Description
unique_key	Unique service-request identifier
created_date	Date and time the request was created
closed_date	Recorded closure date and time
agency / agency_name	Agency code and name
complaint_type	Main complaint category
descriptor	Detailed issue description
incident_zip	Incident ZIP code
borough	Borough
latitude / longitude	Geographic coordinates where available
status	Recorded request status


Derived fields
Derived field	Purpose
resolution_days	Elapsed time between creation and closure
backlog_age_days	Age of a record without a closure date as of 1 January 2026
backlog_aging	Age band: 0–7, 8–30, 31–60, Above 60 Days
invalid_resolution_time	Flags negative resolution durations
status_date_mismatch	Flags non-Closed records with a populated closure date
is_resolved	True when status is Closed
is_unresolved	True when status is not Closed
is_backlog	True when closed_date is blank


KPI definitions
- Total Requests: Count of all records in the selected dataset.
- Resolved Requests: Count of records whose status is Closed.
- Resolution Rate: Resolved Requests divided by Total Requests.
- Backlog Requests: Count of records with a blank closed_date.
- Backlog Above 60 Days: Backlog records older than 60 days as of 1 January 2026.
Important: Status-based unresolved requests and closure-date-based backlog requests are different measures. They should not be treated as interchangeable.
9. Data Quality and Validation
Validation check	Result
Total records	156,034
Distinct request IDs	156,034
Duplicate request IDs	0
Missing created dates	0
Missing closure dates	5,994
Non-Closed status with a populated closure date	840
Negative calculated resolution durations	833


Request status distribution
Status	Requests
Closed	149,200
Pending	4,298
Assigned	1,612
Open	536
Started	363
In Progress	23
Unspecified	2
Total	156,034


Backlog ageing
Age band (reference: 1 Jan 2026)	Requests
0–7 Days	143
8–30 Days	360
31–60 Days	409
Above 60 Days	5,082
Total	5,994


Key data-quality interpretation: 5,994 records have no closure date, while 6,834 records have a status other than Closed. These counts differ because 840 non-Closed records nevertheless contain a closure date. The project preserves and reports these exceptions rather than silently changing the source records.
10. SQL Analysis and Key Results
10.1 Complaint volume by category
Complaint category	Requests	Share
Street Condition	70,330	45.07%
Street Light Condition	33,835	21.68%
Sewer	26,062	16.70%
Sidewalk Condition	25,807	16.54%
Total	156,034	100.00%


Street Condition is the largest category. Street Condition and Street Light Condition together account for approximately 66.75% of requests.
10.2 Requests by borough
Borough	Requests	Share
Queens	53,530	34.31%
Brooklyn	43,337	27.77%
Manhattan	24,905	15.96%
Bronx	19,181	12.29%
Staten Island	14,787	9.48%
Unspecified	294	0.19%


Queens and Brooklyn together account for approximately 62.08% of recorded requests.
10.3 Monthly request volume
Month	Requests
January	11,062
February	11,208
March	13,605
April	13,196
May	14,648
June	14,037
July	14,066
August	13,606
September	13,332
October	14,447
November	11,317
December	11,510


May recorded the highest monthly volume (14,648), while January recorded the lowest (11,062). March had the largest month-over-month increase (+21.39%), while November had the largest decrease (-21.67%). A decrease in reported requests should not automatically be interpreted as improved infrastructure or service performance.
10.4 Resolution performance by complaint category
Category	Closed	Resolution rate	Average valid days	Median valid days
Street Light	31,503	93.11%	28.92	9.24
Sidewalk	25,784	99.91%	11.97	7.87
Street Condition	66,847	95.05%	2.89	1.08
Sewer	25,066	96.18%	2.73	0.21


Negative durations are excluded from valid average and median resolution-time calculations, but the affected records remain in the dataset and are disclosed in the validation results.
10.5 Agency-level comparison
Agency	Requests	Closed	Resolution rate	Backlog	Average valid days
DOT	129,972	124,134	95.51%	5,004	11.38
DEP	26,062	25,066	96.18%	990	2.73


Agency comparisons require caution because agencies handle different service types and case mixes. These figures should not be treated as a direct efficiency ranking.
10.6 Backlog ageing by category
Category	0–7 days	8–30 days	31–60 days	Above 60 days
Sewer	25	64	44	857
Sidewalk	5	2	8	8
Street Condition	85	151	199	3,042
Street Light	28	143	158	1,175


Street Condition contributes the largest number of backlog records above 60 days (3,042).
10.7 Top ZIP-code request volumes
ZIP code	Requests
10314	2,668
10306	2,171
11101	2,122
11385	2,120
11234	2,104
10312	2,070


These are raw request counts. They do not prove that the listed ZIP codes have objectively worse infrastructure.
11. Power BI Dashboard
The interactive report is organized into five pages. Add exported screenshots of each page to the images/ folder in this repository and use the filenames shown below.
Page 1 – Executive Overview
Presents headline KPIs: Total Requests, Resolved Requests, Resolution Rate, Backlog Requests, and Backlog Above 60 Days. Supporting visuals show monthly trends, complaint-category volume, and borough distribution.

Screenshot – add your Power BI Page 1 image below:

<details>
<summary>Screenshot instructions</summary>Save the exported screenshot as page1-executive-overview.png inside the repository's images folder.
</details>Page 2 – Complaint and Trend Analysis
Explores monthly request volume, month-over-month change, category trends, and category distribution across boroughs.

Screenshot – add your Power BI Page 2 image below:

Page 3 – Resolution and Backlog Analysis
Compares average and median valid resolution time, resolution rates by category, backlog ageing, borough backlog, and agency-level indicators.

Screenshot – add your Power BI Page 3 image below:

Page 4 – Geographic Hotspot Analysis
Ranks ZIP codes and boroughs by recorded request volume and compares complaint-category mix across the top 10 ZIP codes. A map visual was not used because the Power BI tenant setting restricted Azure Maps processing outside the permitted geographic region. Bar and column charts were used as an alternative.

Screenshot – add your Power BI Page 4 image below:

Page 5 – Data Quality and Validation
Displays missing closure dates, status-date mismatches, negative resolution durations, duplicate request IDs, and the request-status distribution.

Screenshot – add your Power BI Page 5 image below:

12. Key Findings
1. Demand concentration: Street Condition represents 45.07% of selected requests. Street-related categories dominate the dataset.
2. Geographic concentration: Queens and Brooklyn together account for approximately 62.08% of requests.
3. Monthly variation: May has the highest request volume, while January has the lowest.
4. Resolution performance: The overall status-based resolution rate is 95.62%.
5. Longer resolution times: Street Light Condition has the highest average valid resolution duration at 28.92 days and a median of 9.24 days.
6. Ageing backlog: 5,082 of 5,994 records without closure dates are above 60 days old (84.78%).
7. Data-quality exceptions: 840 status-date mismatches and 833 negative resolution durations were identified; no duplicate request IDs were detected.
13. Business Recommendations
13.1 Introduce age-based backlog triage
Prioritize review of records older than 60 days. Assign owners, verify case notes, and determine whether work remains active, was completed but not updated, or requires escalation.
13.2 Review Street Light resolution delays
Investigate long-duration Street Light cases by descriptor, location, work type, and agency workflow. Identify potential causes such as parts availability, contractor scheduling, access constraints, or delayed status updates.
13.3 Plan resources around demand patterns
Use category volumes and monthly trends to inform staffing and maintenance planning. Validate monthly patterns across multiple years before treating them as recurring seasonality.
13.4 Use normalized geographic indicators
Supplement raw request counts with measures such as requests per 1,000 residents, per kilometre of road, per 1,000 streetlights, or per square kilometre. Use aligned population and asset data.
13.5 Improve status and closure-date governance
Define consistent rules for request closure, require closure dates for closed records, and investigate non-Closed records with populated closure dates and negative durations.
13.6 Establish repeatable reporting
Refresh the dataset on a defined schedule, rerun documented SQL queries, validate results, and compare indicators with previous reporting periods.
14. Limitations
- The analysis covers 2025 records and is not a live operational report.
- Backlog ageing uses a fixed reference date of 1 January 2026.
- There are 5,994 records without closure dates, 840 status-date mismatches, and 833 negative calculated resolution durations.
- Geographic comparisons use raw counts and are not normalized by population or infrastructure assets.
- A service request is an administrative report, not a verified engineering assessment.
- A Closed status does not establish resident satisfaction, repair quality, or compliance with a service-level target.
- Agency comparisons are affected by different service types and case complexity.
- Azure Maps was unavailable under the applicable Power BI tenant setting, so geographic rankings are shown with charts instead.
15. Repository Structure
A suggested GitHub repository structure:
Urban-Infrastructure-Service-Intelligence/
│
├── README.md
├── data/
│   └── urban_infrastructure_311_cleaned.csv
│
├── database/
│   └── urban_infrastructure.db
│
├── sql/
│   └── analysis_queries.sql
│
├── SQL_Analysis_Results/
│   └── (exported SQL result CSV files)
│
├── images/
│   ├── page1-executive-overview.png
│   ├── page2-complaint-trend-analysis.png
│   ├── page3-resolution-backlog-analysis.png
│   ├── page4-geographic-hotspot-analysis.png
│   └── page5-data-quality-validation.png
│
└── report/
    └── Urban_Infrastructure_Service_Intelligence_Project_Report.pdf
Only include files that you are permitted to share. If the raw or cleaned dataset is too large or subject to source terms, provide the source link and preparation instructions instead of committing it.
16. How to Reproduce the Analysis
1. Open the NYC Open Data dataset and obtain the 2025 records for the four selected complaint categories.
2. Use Python and Pandas to inspect, clean, transform, and export the analytical dataset.
3. Load the prepared data into SQLite using the table name service_requests.
4. Run the documented SQL queries to reproduce category, monthly, resolution, backlog, agency, borough, and ZIP-code summaries.
5. Import the prepared data and analysis outputs into Power BI Service.
6. Recreate or refresh the five dashboard pages.
7. Validate the KPI values against the SQL outputs and document any differences.
The exact source extract and transformations should be retained with the project so that the analysis can be reproduced consistently.
17. Conclusion and Future Scope
This project demonstrates an end-to-end Business Intelligence workflow using municipal 311 service-request data. Python and Pandas were used for preparation, SQL and SQLite for structured analysis, and Power BI Service for interactive reporting.

The analysis highlights high request volumes in Street Condition, geographic concentration in Queens and Brooklyn, longer average resolution times for Street Light Condition, and a backlog snapshot dominated by records older than 60 days. Data-quality findings show why status and closure-date governance must be considered alongside operational KPIs.
Future scope
- Extend the analysis to multiple years to identify longer-term patterns.
- Integrate population, road-length, streetlight, and sidewalk asset data.
- Add service-level targets, first-response time, reopen rates, and resident satisfaction.
- Automate data refresh and validation.
- Introduce approved geospatial analysis where permitted.
- Explore predictive backlog-risk modelling after validating sufficient historical data.
18. Data Source and References
- NYC Open Data – 311 Service Requests from 2020 to Present:
  https://data.cityofnewyork.us/Social-Services/311-Service-Requests-from-2020-to-Present/erm2-nwe9
- NYC Open Data API:
  https://data.cityofnewyork.us/resource/erm2-nwe9.json
Project Deliverables
- Cleaned analytical dataset
- SQLite database
- SQL analysis queries and exported result tables
- Five-page Power BI dashboard
- PDF project report
- GitHub project documentation
Author
Name: [Your Name]
Role: Data Analyst / Business Intelligence Project
Tools: Python | SQL | SQLite | Power BI | Excel
This project is an independent analytical portfolio project using publicly available NYC Open Data. The findings are descriptive and should be interpreted within the stated scope and limitations.
