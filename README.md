# Airline Delay Analysis Dashboard

## Project Overview
This project analyzes flight delay patterns and cancellation reasons using 2008 airline operational data. The analysis identifies key factors affecting flight performance, including delay causes, seasonal trends, and airport-specific issues.

## Dataset Information
- **Source**: Airline Delay Causes (Kaggle)
- **File**: DelayedFlights.csv
- **Records**: 1,936,758 flights
- **Time Period**: 2008 (full year)
- **Coverage**: US domestic flights across 20 airlines and 548 airports

## Key Metrics
- **Total Flights**: 1.63M
- **Delayed Flights (Arrival)**: 1.46M (89.5%)
- **Delayed Flights (Departure)**: 1.63M (100%)
- **Cancelled Flights**: 548
- **Diverted Flights**: 6.33K
- **Active Airlines**: 20

## Data Structure

### Fact Table: Flights
Contains all flight records with timing, delay, and operational data.

**Key Columns**:
- Flight identifiers: FlightNum, TailNum, UniqueCarrier
- Scheduled times: CRSDepTime, CRSArrTime, CRSElapsedTime
- Actual times: DepTime, ArrTime, ActualElapsedTime
- Delays: DepDelay, ArrDelay
- Delay causes: CarrierDelay, WeatherDelay, NASDelay, SecurityDelay, LateAircraftDelay
- Ground operations: TaxiIn, TaxiOut
- Status flags: Cancelled, Diverted

### Dimension Tables
- **dim_date**: Time hierarchy (Year, Month, Day, MonthName, DayName)
- **dim_airport**: Airport codes and locations
- **dim_carrier**: Airline information

## Data Cleaning Process

### Handled Issues
1. **Missing Values**
   - ArrTime, AirTime, ArrDelay: NaN for cancelled/diverted flights (expected)
   - Delay cause columns: Filled with 0 for on-time flights
   - TailNum: Minimal nulls (5), filled with 'Unknown'

2. **Data Quality**
   - Removed rows with invalid time formats (hour > 23)
   - Validated HHMM format consistency
   - Cross-checked Cancelled flag with cancellation codes

3. **Feature Engineering**
   - Created delay severity categories: OnTime, Minor, Moderate, Severe
   - Extracted time of day: Morning (0-11), Afternoon (12-17), Evening (18-23)
   - Added seasonal classification: Winter, Spring, Summer, Fall
   - Flagged weekend flights
   - Identified primary delay cause per flight

## Dashboard Pages

### Page 1: Delays Overview
![Delays Overview Dashboard]("D:\ITI\Power BI\Day6\Dashboards\Delayed.jpg")

**Key Visualizations**:
- KPI Cards (Top Row):
  - Total Flights: 1.63M
  - Unique Carriers: 20
  - Cancelled Flights: 548
  - Diverted Flights: 6.33K
  - Delayed Arrivals: 1.46M
  - Delayed Departures: 1.63M

- Delayed Flights over 2008 (Line Chart):
  - Tracks both arrival and departure delays throughout the year
  - Shows peaks in January/February and December
  - Departure delays consistently higher than arrival delays

- Amount of Delay by Causes (Horizontal Bar Chart):
  - Late Aircraft Delay: 28.17M minutes (largest contributor)
  - Carrier Delay: 19.09M minutes
  - NAS Delay: 16.33M minutes
  - Security Delay: 0.08M minutes

- Arrival Delay by Destination Airport (Bar Chart):
  - Top airports: ORD, ATL, DFW (highest delays)
  - Shows airport congestion patterns
  - Enables filtering by origin airport

- Departure Delay by Destination Airport (Bar Chart):
  - Similar pattern to arrival delays
  - ATL and ORD lead in departure delays
  - Useful for identifying problematic routes

**Filters**: Destination Airport, Origin Airport, Delays toggle

### Page 2: Cancellations Analysis
![Cancellations Dashboard]("D:\ITI\Power BI\Day6\Dashboards\Cancelled.jpg")

**Key Visualizations**:
- Cancelled Flights over 2008 (Line Chart):
  - Sharp increase from October onwards
  - Peak in December (over 400 cancellations)
  - Minimal cancellations January through September
  - Indicates seasonal winter weather impact

- Cancellation Reason (Pie Chart):
  - Weather: 269 flights (49.09%) - Primary cause
  - Carrier: 210 flights (38.32%)
  - NAS: 69 flights (12.59%)
  - Shows weather dominates cancellation reasons

- Cancelled by Destination Airport (Bar Chart):
  - ORD (Chicago) leads with ~65 cancellations
  - ATL, EWR, SEA follow with 20+ cancellations each
  - Identifies most impacted destination airports

- Cancelled by Origin Airport (Bar Chart):
  - ORD (Chicago) highest with ~80 cancellations
  - IAH, ATL, DEN in top positions
  - Shows which airports have highest cancellation rates

**Filters**: Destination Airport, Origin Airport, Cancelled toggle

### Page 3: Diversions Analysis
![Diversions Dashboard]("D:\ITI\Power BI\Day6\Dashboards\Diverted.jpg")
**Key Visualizations**:
- Diverted Flights over 2008 (Line Chart):
  - Volatile pattern with peaks in January, May, and December
  - Summer months (June-September) show lower diversion rates
  - Annual total: 6.33K diverted flights

- Diverted by Origin (Bar Chart):
  - ORD dominates with ~350 diversions
  - IAH, ATL, and DEN in top positions
  - Shows which airports divert most flights

- Diverted by Destination (Bar Chart):
  - More distributed than cancellations
  - ORD still leads but gap smaller than cancellations
  - Indicates better backup options for diverted flights

**Filters**: Destination Airport, Origin Airport, Diverted toggle

### Key Insights from Dashboards

**Delay Patterns**:
- Peak Delay Months: December shows spike in both delays and cancellations
- Summer Anomaly: September shows lowest delays (post-summer recovery)
- Primary Delay Cause: Late aircraft delays (28.17M minutes total)
- Major Hubs: ORD, ATL, DFW consistently show highest delays

**Cancellation Insights**:
- Winter Weather Impact: 49% of cancellations weather-related
- Seasonal Trend: Cancellations spike in Q4 (October-December)
- Airport Concentration: ORD accounts for highest cancellations (both origin/dest)
- Carrier Responsibility: 38% of cancellations due to airline operations

**Diversion Patterns**:
- Distributed Impact: Diversions spread across more airports than cancellations
- Spring/Summer Peaks: May and summer months show elevated diversion rates
- Economic Efficiency: Airlines divert rather than cancel when possible
- Relief Airports: Multiple diversions by destination show alternative airport usage

## How to Use

### In Power BI
1. Load DelayedFlights_Cleaned.csv as main fact table
2. Import dimension tables (dim_date, dim_airport, dim_carrier)
3. Create relationships:
   - Flights.Month → dim_date.Month
   - Flights.Origin → dim_airport.Code
   - Flights.Dest → dim_airport.Code
   - Flights.UniqueCarrier → dim_carrier.CarrierCode
4. Use slicers for filtering by destination, origin, and cancellation status
5. Drill down through date hierarchy for month and day analysis

### Filtering
- **Destination Airport**: Filter to specific destinations to see route-specific patterns
- **Origin Airport**: Identify which departure airports have worst performance
- **Cancelled**: Toggle between all flights, cancelled only, or operational only
- **Delays**: Switch between arrival delay and departure delay analysis

## Technical Details

### Tools Used
- **Python 3.x**: Data cleaning and exploration
- **Pandas**: Data manipulation and transformation
- **NumPy**: Numerical operations
- **Power BI**: Visualization and dashboarding

### Python Libraries
```
pandas
numpy
matplotlib
seaborn
```

### Power BI Features
- Dynamic filtering with slicers
- Date hierarchy for drill-down analysis
- Conditional formatting on KPI cards
- Custom measures for delay calculations
- Color-coded severity indicators

## Data Quality Notes

### Known Limitations
- Departure delays show 100% occurrence (potential data quality issue)
- All data from single year (2008) limits trend analysis
- Some records lack specific delay cause attribution

### Assumptions
- Positive delay values indicate flights were late
- Negative delay values indicate flights arrived/departed early
- Missing delay causes (NaN) for cancelled/diverted flights are expected
- TimeKey in dimension tables enables fast lookups

## Files Included
- `DelayedFlights_Cleaned.csv`: Main dataset (ready for Power BI)
- `dim_date.csv`: Date dimension table
- `dim_airport.csv`: Airport dimension table (if created)
- `dim_carrier.csv`: Airline dimension table (if created)
- `EDA.ipynb`: Exploratory data analysis notebook
- `README.md`: This file

## Future Enhancements
- Add predictive model for delay forecasting
- Implement real-time data refresh
- Create drill-through reports for route-level analysis
- Add seasonal decomposition analysis
- Develop mobile-friendly dashboard version

## Contact & Support
For questions or issues with the analysis, refer to the EDA notebook for detailed methodology and exploratory findings.

---
**Analysis Date**: 2008 Airline Operations Data
**Last Updated**: 2025
