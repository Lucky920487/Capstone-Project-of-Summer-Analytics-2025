# Capstone-Project-of-Summer-Analytics-2025
# Capstone Project: Dynamic Pricing for Urban Parking Lots

**Program**: Summer Analytics 2025  
**Organized by**: Consulting & Analytics Club × Pathway  
**Prepared by**: Lucky Kumari

---

## 📌 Introduction

Urban parking lots are high-demand resources that often suffer from inefficiencies due to static pricing. This project addresses that challenge through a dynamic pricing system that adjusts in real-time using simulated streaming data and machine learning logic. We use [Pathway](https://pathway.com/developers/) to simulate real-time input and pricing updates.

---

## 📊 Dataset Overview

- **Locations**: 14 parking spaces
- **Time Span**: 73 days × 18 time intervals/day (8:00 AM to 4:30 PM)
- **Columns**:
  - `Latitude`, `Longitude`
  - `Capacity`, `Occupancy`, `Queue Length`
  - `Vehicle Type` (car, bike, truck)
  - `Traffic`, `Special Day Indicator`
  - `Timestamp` = `Date + Time`

---

## 🔍 Data Preprocessing

- Combined `LastUpdatedDate` and `LastUpdatedTime` into a single `timestamp`.
- Renamed fields to: `lot_id`, `occupancy`, `queue_length`, `vehicle_type`, etc.
- Exported cleaned data to `cleaned_dataset.csv` for streaming.

---

## 🤖 Model Implementation

### 🔹 Model 1: Baseline Linear Pricing
- Formula: `price = price + α × (occupancy / capacity)`
- α = 2.0, Base price = $10
- Purpose: Simple benchmark

### 🔹 Model 2: Demand-Based Pricing
- Factors: Occupancy, Queue, Traffic, Event Day, Vehicle Type
- Price = `base_price × (1 + λ × demand)`
- Demand is normalized and weighted

### 🔹 Model 3: Competitive Pricing Model
- Uses `geopy` to compute distances between lots
- Adjusts price based on competitor pricing in nearby lots
- Encourages optimal use and simulates market behavior

---

## 🚦 Real-Time Streaming with Pathway

- `main.py` reads `cleaned_dataset.csv` using `pathway.io.csv.read`
- Logic wrapped in `@pw.udf`
- Results streamed and written to `output.jsonl`
- Schema defined via `pw.Schema`

---

## 📈 Visualization with Bokeh

- `visualize.py` plots dynamic price over time for each lot
- Uses `ColumnDataSource.stream()` to simulate live pricing

To run the dashboard:
```bash
bokeh serve visualize.py

