# Airlines-Data-Analysis


## 📌 Project Overview
This project provides an in-depth analysis of airline seating data, including booked seats, total available seats, and occupancy rates per aircraft. The goal is to understand flight occupancy patterns, identify trends, and provide insights for airline optimization and revenue management.

## 📂 Dataset
The dataset consists of three main tables:
- **`boarding_passes`**: Contains details of passenger seat allocations and flight IDs.
- **`flights`**: Holds essential flight details such as aircraft codes, flight IDs, and schedules.
- **`seats`**: Provides the total number of seats available for each aircraft type.

## ⚙️ Installation & Setup
### Prerequisites
Ensure you have the following installed:
- jupyter notebook
- SQLite3
- Pandas, Matplotlib, and Seaborn

### Steps to Install
1. **Clone the repository**
   ```sh
   git clone https://github.com/yourusername/Airlines-Data-Analysis-Repository.git
   cd Airlines-Data-Analysis-Repository
   ```
2. **Install dependencies**
   ```sh
   pip install pandas matplotlib seaborn sqlite3
   ```
3. **Set up the database connection**
   Ensure your SQLite database is correctly set up and modify `connection` in the script accordingly.

## 🔍 Data Analysis
### SQL Query Used:
This SQL query calculates the **average booked seats, total available seats, and occupancy rate per aircraft type**.
```sql
SELECT
    a.aircraft_code,
    AVG(a.seats_count) AS booked_seats,
    b.num_seats,
    AVG(a.seats_count * 1.0 / b.num_seats) AS occupancy_rate
FROM (
    SELECT
        flights.aircraft_code,
        flights.flight_id,
        COUNT(*) AS seats_count
    FROM boarding_passes
    INNER JOIN flights 
        ON boarding_passes.flight_id = flights.flight_id
    GROUP BY flights.aircraft_code, flights.flight_id
) AS a
INNER JOIN (
    SELECT
        aircraft_code,
        COUNT(*) AS num_seats
    FROM seats
    GROUP BY aircraft_code
) AS b
ON a.aircraft_code = b.aircraft_code
GROUP BY a.aircraft_code;
```

## 📊 Visualizations
The analysis includes the following visualizations:
1. **Booked vs. Total Seats** - A bar chart comparing booked seats and total seats per aircraft.
2. **Occupancy Rate per Aircraft** - A line chart showing the percentage of occupied seats.
3. **Combined Chart (Seats & Occupancy Rate)** - A dual-axis visualization displaying both metrics.

### 📌 Example Visualization Code
```python
import sqlite3
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load Data
connection = sqlite3.connect("your_database.db")
df = pd.read_sql_query("SELECT * FROM your_query_here", connection)

# Bar Chart for Booked vs. Total Seats
plt.figure(figsize=(12, 6))
sns.barplot(data=df, x="aircraft_code", y="booked_seats", color="blue", label="Booked Seats")
sns.barplot(data=df, x="aircraft_code", y="num_seats", color="red", alpha=0.5, label="Total Seats")
plt.xlabel("Aircraft Code")
plt.ylabel("Number of Seats")
plt.title("Booked vs. Total Seats per Aircraft")
plt.legend()
plt.xticks(rotation=45)
plt.show()
```

## 📈 Insights & Observations
- Some aircraft types consistently have higher occupancy rates, indicating high demand.
- Low-occupancy aircraft could indicate underutilized routes or scheduling inefficiencies.
- Visualizing occupancy rates helps airlines make data-driven decisions for better seat allocation.

## 🚀 Future Improvements
- **Real-time data integration**: Fetch live data for more accurate analysis.
- **Machine Learning Integration**: Predict future seat demand and optimize flight scheduling.
- **SQL Query Optimization**: Improve query performance for large datasets.

## 🤝 Contributing
Contributions are welcome! If you’d like to improve this project:
1. Fork the repository.
2. Create a new branch (`git checkout -b feature-branch`).
3. Commit your changes (`git commit -m "Added new feature"`).
4. Push to the branch (`git push origin feature-branch`).
5. Open a Pull Request.

## 📜 License
This project is licensed under the **MIT License**.

## 📬 Contact
For any queries, reach out via [sushantraj893@gmail.com].
