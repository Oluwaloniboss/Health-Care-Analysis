Healthcare Dashboard – Power BI
Overview
A Power BI dashboard designed for healthcare administrators to visualize patient flow, admission patterns, bed utilization, and discharge efficiency. Built to support data-driven decisions and resource optimization.

Features
Key Metrics – Total Patients, YTD Total, Bed Occupancy %, Avg. Length of Stay

Admission/Discharge Analysis – Monthly trends, cards for totals

Admission Heatmap – By hour of day and day of week

Department & Diagnosis Breakdown – Bar charts for top departments/diagnoses

Weekday vs Weekend Comparison – Donut chart showing admission proportions

Interactive Slicers – Date range, AM/PM period, and toggle between admissions/discharges

Data Requirements
The dashboard expects a dataset containing at least:

Patient ID

Admission Date & Time (as decimal hours, e.g., 11.34 for 11:20)

Discharge Date & Time (optional but needed for length of stay)

Department

Diagnosis

Bed ID (optional for bed occupancy calculation)

Measures (DAX)
All measures are grouped in a Measures folder:

Total Patients = COUNTROWS(Admissions)

Total Patients YTD = TOTALYTD([Total Patients], 'Date'[Date])

Bed Occupancy % = DIVIDE([Total Patients], [Total Available Beds], 0) * 100

Avg Length of Stay = AVERAGEX(FILTER(Admissions, Admissions[Discharge Date] <> BLANK()), DATEDIFF(Admissions[Admission Date], Admissions[Discharge Date], DAY))

Total Admissions = COUNTROWS(Admissions)

Total Discharges = COUNTROWS(FILTER(Admissions, Admissions[Discharge Date] <> BLANK()))

Visuals Included
KPI Cards – Four top-level metrics.

Admission Heatmap – Matrix (rows = weekday, columns = hour) with conditional formatting.

Monthly Admissions & Discharges – Line and clustered column chart.

Admissions by Department – Horizontal bar chart.

Admissions by Diagnosis – Horizontal bar chart.

Weekday vs Weekend Donut – Shows percentage split.

Admission vs Discharge Trend – Daily line chart.

Slicers – Date, AM/PM, and view toggle (Admissions/Discharges).

How to Use
Load Data – Import your dataset (CSV/Excel) into Power BI.

Transform Data – Round time column to nearest hour, replace errors, create a Date table (with Year, Month, Weekday, WeekType).

Create Relationships – Connect Date table to Admission Date.

Add Measures – Use the DAX formulas above.

Build Visuals – Arrange as described, apply slicers.

Publish – (Optional) to Power BI Service.

Requirements
Power BI Desktop (free)

Basic understanding of DAX and data modeling
