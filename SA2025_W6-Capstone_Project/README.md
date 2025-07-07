# 🚗 Dynamic Real-Time Parking Pricing System

**Summer Analytics 2025 – Week 6 Project**  

---

## 📌 Problem Overview

We are tasked with building a real-time pricing model for smart city parking lots. The price must dynamically respond to occupancy, demand, and competition. The goal is to implement:

- Multiple pricing models (basic to advanced)
- A real-time simulation pipeline using **Pathway**
- Visualizations using **Bokeh**
- Clear reporting of assumptions, demand modeling, and logic


---

## 🛠️ Tech Stack Used

- **Python 3.11** — primary programming language
- **Pandas** — for data manipulation and preprocessing
- **NumPy** — for numerical operations
- **Pathway** — for real-time streaming and processing
- **Bokeh** — for interactive data visualization
- **Google Colab** — for cloud-based development and execution
- **Git & GitHub** — for version control and submission

---

---

## 🧱 Architecture & Workflow Explanation

Our system simulates a real-time smart parking pricing engine using a three-stage architecture:

1. **Data Ingestion**:  
   - A sample of parking lot sensor data is read from a CSV.
   - This simulates incoming real-world traffic, occupancy, and queue data.

2. **Stream Processing with Pathway**:  
   - Pathway reads the data in streaming mode and processes it row-by-row.
   - A pricing UDF calculates price dynamically using a demand-based model that considers occupancy, queue, traffic, special day, and vehicle type.

3. **Real-Time Output & Visualization**:  
   - The computed prices are saved to `stream_output.csv`.
   - We then use Bokeh to visualize the pricing trends per lot in an interactive time series chart.

This architecture closely mimics how an actual smart city system would run in production — processing live data and adjusting prices in real time based on environmental and behavioral factors.

![Architecture Diagram](architecture.png)


---


## 🧠 Models Implemented

### 1️⃣ Model 1: Linear Incremental Pricing

\[
P_{t+1} = P_t + \alpha \cdot \left(\frac{\text{Occupancy}}{\text{Capacity}}\right)
\]

- Rule-based and accumulative
- No consideration of external environment
- Used for baseline comparison

---

### 2️⃣ Model 2: Demand-Based Dynamic Pricing

\[
\text{Demand} = \alpha \cdot \text{OccupancyRatio} + \beta \cdot \text{QueueLength} + \gamma \cdot \text{Traffic} + \delta \cdot \text{SpecialDay} + \epsilon \cdot \text{VehicleType}
\]

\[
\text{Price} = 10 \cdot (1 + \lambda \cdot \text{NormalizedDemand})
\]

- Dynamic and bounded between \$10–\$20
- Smooth and responsive to traffic/demand context

---

### 3️⃣ Model 3: Competitive Pricing

- Compares real-time prices of **neighboring lots**
- If nearby lots are cheaper & you're full → lower your price
- If you’re cheaper & have space → increase price
- Uses **Haversine** distance to detect proximity

---

## 🌀 Real-Time Simulation with Pathway

We simulate continuous input using [Pathway](https://github.com/pathwaycom/pathway):

- Input: `stream_input.csv` (sample of 1000 rows)
- Streaming mode: `mode="streaming"` with real-time schema
- Output: `stream_output.csv` containing timestamps, lot IDs, and prices
- Errors handled with default fallback values (e.g., Traffic = 0.3 if missing)

---

## 📊 Interactive Visualization with Bokeh

We visualize the price evolution of multiple parking lots using **Bokeh**:

- Interactive line chart with pan/zoom
- Hover tool shows `Lot` and `Price`
- Toggle visibility using legend
- Helps understand model responsiveness over time

📷 *Bokeh Chart Sample:*

![Real-Time Price Chart](bokeh_chart.png)  <!-- Replace with actual screenshot filename -->

---

## 🧾 Final Report Section

### 🔍 Demand Function Weights

- OccupancyRatio → 0.4  
- QueueLength → 0.3  
- TrafficWeight → 0.2  
- IsSpecialDay → 0.5  
- VehicleTypeWeight → 0.1  
- Scaling parameter `λ = 1.0`

### 🔐 Assumptions

- Missing traffic and vehicle type weights default to 0.3 and 1.0
- Base price is \$10; prices bounded between \$5 and \$20
- At most 10 lots plotted for clarity
- Haversine radius for competition: 500 meters

---

## ✅ Submission Checklist

| Requirement                                     | Status |
|------------------------------------------------|--------|
| Well-commented notebook                        | ✅ Done |
| Models 1, 2, 3 implemented                      | ✅ Done |
| Report: demand, assumptions, competition logic | ✅ Done |
| Real-time simulation with Pathway              | ✅ Done |
| Bokeh visualization (notebook & report)        | ✅ Done |
| GitHub README with full documentation          | ✅ Done |

---

## 🏁 Conclusion

This project showcases a full-stack simulation of dynamic pricing in smart cities — combining algorithmic modeling, streaming data, and interactive visual analytics. Built using Python, Pandas, Pathway, and Bokeh.

🚀 Ready for evaluation!
