# Healthcare Patient Flow & Operations Dashboard
## 📊 Project Overview
This repository contains a Power BI dashboard designed to track and analyze patient admissions, discharges, bed utilization, and operational trends in a healthcare setting. The dashboard transforms raw admission records into actionable insights, enabling hospital administrators and department heads to monitor performance, identify bottlenecks, and make data-driven decisions.

The project follows a structured approach based on a provided business problem, covering key performance indicators (KPIs), admission patterns, departmental breakdowns, and temporal trends.

## 🎯 Objective
- **Monitor Core Metrics:** Track total patients, year-to-date admissions, average length of stay, and bed occupancy percentage.
- **Analyze Admission Patterns:** Identify peak admission hours, busy weekdays, and seasonal trends.
- **Evaluate Department & Diagnosis Load:** Understand which departments and diagnoses drive admissions.
- **Compare Weekday vs Weekend Activity:** Assess differences in admission volumes to guide staffing and resource planning.
- **Deliver an Interactive Dashboard:** Provide a user-friendly interface with slicers for date, time of day (AM/PM), and view toggles (admissions/discharges).

## 🛠️ Tools & Technologies
- **Data Visualization:** Microsoft Power BI Desktop
- **Data Modeling:** DAX (Data Analysis Expressions)
- **Data Transformation:** Power Query (M language)
- **Data Source:** Hospital admission records (CSV/Excel)

## 🔑 Key Features & Measures
### Data Modeling
- **Star Schema:** Fact table (`Admissions`) connected to dimension tables (`Date`, `Department`, `Diagnosis`).
- **Date Table:** Created using `CALENDARAUTO()` with columns for Year, Month, Month Name, Week Number, Weekday, and a custom `WeekType` (Weekday/Weekend).
- **Measures Table:** A dedicated table to store all DAX measures for improved organization and performance.

### Core DAX Measures
- `Total Patients = COUNTROWS(Admissions)`
- `Total Patients YTD = TOTALYTD([Total Patients], 'Date'[Date])`
- `Bed Occupancy % = DIVIDE([Total Patients], 200, 0) * 100`  *(assuming 200 available beds)*
- `Avg Length of Stay = AVERAGEX(FILTER(Admissions, Admissions[Discharge Date] <> BLANK()), DATEDIFF(Admissions[Admission Date], Admissions[Discharge Date], DAY))`
- `Total Admissions = COUNTROWS(Admissions)`
- `Total Discharges = CALCULATE(COUNTROWS(Admissions), NOT(ISBLANK(Admissions[Discharge Date])))`
- `Admission Hour` (calculated column in Power Query): rounded from decimal time (e.g., 11.34 → 11)

### Dashboard KPIs
1. **Overview Page:**  
   - KPI cards: Total Patients, Total Patients YTD, Bed Occupancy %, Avg Length of Stay  
   - Admission heatmap (hour vs. weekday)  
   - Weekday vs Weekend donut chart  
   - Monthly admissions & discharges trend (line and clustered column chart)

2. **Department & Diagnosis Analysis:**  
   - Bar charts: Admissions by Department, Admissions by Diagnosis  
   - Drill-through to detailed patient list

3. **Trends & Comparisons:**  
   - Daily admission vs discharge line chart  
   - Slicers: Date range, AM/PM period (based on `Admission Hour`), and a what‑if parameter to toggle between admissions/discharges

4. **Geographic & Payer Insights** (if data available):  
   - Map view of admissions by state/city  
   - Insurance vs out-of-pocket breakdown

## 🚀 How to Use
1. **Load Data:** Import your CSV/Excel file using `Get Data` in Power BI Desktop.
2. **Transform Data (Power Query):**  
   - Round `Admission Time` to the nearest hour:  
     `= Table.AddColumn(#"Previous Step", "Admission Hour", each Number.Round([Admission Time]), Int64.Type)`  
   - Replace errors (e.g., nulls in key columns).  
   - Create a `Date` table if not already present (use DAX below).
3. **Build the Date Table (DAX):**  
   `Date = CALENDAR(MIN(Admissions[Admission Date]), MAX(Admissions[Discharge Date]))`  
   Add calculated columns: `Year`, `Quarter`, `Month`, `Month Name`, `Week Number`, `Weekday`, `WeekType`.
4. **Establish Relationships:** Connect `Date[Date]` to `Admissions[Admission Date]` (active) and optionally to `Discharge Date` (inactive).
5. **Create Measures:** Copy the DAX measures into the model and group them in a folder named "Measures".
6. **Design Visuals:** Arrange the visuals as described in the **Key Features** section.
7. **Add Interactivity:** Insert slicers, configure the AM/PM filter, and set up the what‑if parameter.
8. **Publish:** Save the `.pbix` file and optionally publish to Power BI Service.

## 💡 Insights & Recommendations (Example)
- **Peak Admission Hours:** The heatmap reveals highest admissions on weekdays between 10 AM – 2 PM; consider scheduling additional staff during these windows.
- **Department Load:** Cardiology and Orthopedics account for over 50% of admissions; ensure adequate bed capacity and nursing resources.
- **Length of Stay:** Average stay is 4.2 days; surgical patients tend to stay longer – explore discharge planning improvements.
- **Weekend Admissions:** Only 28% of admissions occur on weekends; staffing could be slightly reduced on Saturdays/Sundays without impacting service.
- **Seasonal Trends:** Admissions spike in January and July; plan for these peaks in budget and staffing cycles.

## 📝 Future Work
- Integrate readmission rates to track quality of care.
- Add predictive analytics to forecast daily admission volumes.
- Include patient satisfaction scores to correlate with operational metrics.
- Automate data refresh via Power BI Gateway for real-time monitoring.

---
