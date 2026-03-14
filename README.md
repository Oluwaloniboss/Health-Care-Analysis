Healthcare Patient Flow & Operations Dashboard

📊 Project Overview

This repository contains a Power BI dashboard designed to track and analyze patient admissions, discharges, bed utilization, and operational trends in a healthcare setting. The dashboard transforms raw admission records into actionable insights, enabling hospital administrators and department heads to monitor performance, identify bottlenecks, and make data-driven decisions.

The project follows a structured approach based on a provided business problem, covering key performance indicators (KPIs), admission patterns, departmental breakdowns, and temporal trends.

🎯 Objective

Monitor Core Metrics: Track total patients, year-to-date admissions, average length of stay, and bed occupancy percentage.

Analyze Admission Patterns: Identify peak admission hours, busy weekdays, and seasonal trends.

Evaluate Department & Diagnosis Load: Understand which departments and diagnoses drive admissions.

Compare Weekday vs Weekend Activity: Assess differences in admission volumes to guide staffing and resource planning.

Deliver an Interactive Dashboard: Provide a user-friendly interface with slicers for date, time of day (AM/PM), and view toggles (admissions/discharges).

🛠️ Tools & Technologies

Data Visualization: Microsoft Power BI Desktop

Data Modeling: DAX (Data Analysis Expressions)

Data Transformation: Power Query (M language)

Data Source: Hospital admission records (CSV/Excel)

🔑 Key Features & Measures

Data Modeling
Star Schema: Fact table (Admissions) connected to dimension tables (Date, Department, Diagnosis).

Date Table: Created using CALENDARAUTO() with columns for Year, Month, Month Name, Week Number, Weekday, and a custom WeekType (Weekday/Weekend).

Measures Table: A dedicated table to store all DAX measures for improved organization and performance.

Core DAX Measures
dax
-- Total Patients
Total Patients = COUNTROWS(Admissions)

-- Total Patients YTD
Total Patients YTD = TOTALYTD([Total Patients], 'Date'[Date])

-- Bed Occupancy % (assuming 200 available beds)
Bed Occupancy % = DIVIDE([Total Patients], 200, 0) * 100

-- Average Length of Stay (days)
Avg Length of Stay = 
AVERAGEX(
    FILTER(Admissions, Admissions[Discharge Date] <> BLANK()),
    DATEDIFF(Admissions[Admission Date], Admissions[Discharge Date], DAY)
)

-- Total Admissions (implicitly same as Total Patients, but defined for clarity)
Total Admissions = COUNTROWS(Admissions)

-- Total Discharges
Total Discharges = 
CALCULATE(
    COUNTROWS(Admissions),
    NOT(ISBLANK(Admissions[Discharge Date]))
)

-- Admission Hour (calculated column in Power Query)
// Rounded from decimal time (e.g., 11.34 → 11)
Dashboard Pages
Overview Page – KPI cards, admission heatmap, weekday/weekend donut, and monthly trend.

Department & Diagnosis Analysis – Bar charts for admissions by department and diagnosis, with drill-through capabilities.

Trends & Comparisons – Daily admission vs discharge line chart, time-based slicers, and a toggle for AM/PM views.

Geographic & Payer Insights (if data available) – Map view of admissions by state/city, insurance vs out-of-pocket breakdown.

Interactive Features
Slicers: Date range, AM/PM period (based on rounded admission hour), and a what‑if parameter to switch between viewing admissions and discharges.

Conditional Formatting: Heatmap cells colored by admission count for quick pattern recognition.

Drill-through: Right-click from a department to see detailed patient list.

🚀 How to Use
Prerequisites
Install Power BI Desktop (free).

Prepare your dataset with the required columns: Patient ID, Admission Date, Admission Time (decimal hours), Discharge Date, Department, Diagnosis, (optional) Bed ID, State, City, Insurance Amount, Out of Pocket.

Steps
Load Data: Import your CSV/Excel file using Get Data.

Transform Data (Power Query):

Round Admission Time to the nearest hour:
= Table.AddColumn(#"Previous Step", "Admission Hour", each Number.Round([Admission Time]), Int64.Type)

Replace errors (e.g., nulls in key columns).

Create a Date table if not already present (use the DAX below or Power Query).

Build the Date Table (DAX):

dax
Date = CALENDAR(MIN(Admissions[Admission Date]), MAX(Admissions[Discharge Date]))
Add calculated columns: Year, Quarter, Month, Month Name, Week Number, Weekday, WeekType.

Establish Relationships: Connect Date[Date] to Admissions[Admission Date] (active) and optionally to Discharge Date (inactive).

Create Measures: Copy the DAX measures into the model and group them in a folder named "Measures".

Design Visuals: Arrange the visuals as described in the Key Features section.

Add Interactivity: Insert slicers, configure the AM/PM filter, and set up the what‑if parameter.

Publish: Save the .pbix file and optionally publish to Power BI Service.

💡 Insights & Recommendations (Example)
Peak Admission Hours: The heatmap reveals highest admissions on weekdays between 10 AM – 2 PM; consider scheduling additional staff during these windows.

Department Load: Cardiology and Orthopedics account for over 50% of admissions; ensure adequate bed capacity and nursing resources.

Length of Stay: Average stay is 4.2 days; surgical patients tend to stay longer – explore discharge planning improvements.

Weekend Admissions: Only 28% of admissions occur on weekends; staffing could be slightly reduced on Saturdays/Sundays without impacting service.

Seasonal Trends: Admissions spike in January and July; plan for these peaks in budget and staffing cycles.

📈 Future Work
Integrate readmission rates to track quality of care.

Add predictive analytics to forecast daily admission volumes.

Include patient satisfaction scores to correlate with operational metrics.

Automate data refresh via Power BI Gateway for real-time monitoring.

📄 License
This project is provided for educational and professional use. Feel free to adapt it to your specific needs.
