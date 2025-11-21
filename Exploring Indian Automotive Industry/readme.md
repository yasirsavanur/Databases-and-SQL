# Exploring Automotive Industry Trends in India

This project uses a real-world used-car dataset to explore trends in the Indian automotive market using SQL. Each row in the dataset represents a car listed for sale in India, with details like model, year, selling price, mileage, fuel type, transmission, ownership history, engine size, power and more. The goal is to treat this as a small analytics problem and answer practical questions about pricing, value and usage patterns.

---

## Dataset

Folder: `Exploring Automotive Industry trends in India`

Files:

- [`Exploring Trends in the Automotive Industry.csv`](Exploring%20Trends%20in%20the%20Automotive%20Industry.csv)  
  Raw used-car listing data with columns such as:
  - `Name` – model/variant
  - `year` – model year
  - `selling_price`
  - `km_driven`
  - `fuel`
  - `seller_type`
  - `transmission`
  - `owner` – first owner, second owner, etc.
  - `mileage`
  - `engine`
  - `max_power`
  - `torque`
  - `seats`

- [`Exploring Trends in the AutomotiveIndustry.sql`](Exploring%20Trends%20in%20the%20AutomotiveIndustry.sql)  
  SQL script with a series of analytical queries written against a table `car_info` in a database `cars_info`.

---

## What this project does

The SQL script is structured as a set of questions on top of the `car_info` table. In short, it:

1. **Aggregates prices by fuel type and other attributes**  
   - For example, calculates the average selling price by `fuel` for manual, first-owner cars.  
   - Uses filters in `WHERE` and groups in `GROUP BY` to understand how specific segments are priced.

2. **Finds efficient cars with more seating capacity**  
   - Computes the average `mileage` per model (`Name`) for cars with more than 5 seats.  
   - Helps answer which larger cars are relatively more fuel-efficient.

3. **Identifies models with large price variation**  
   - Uses `MAX(selling_price) - MIN(selling_price)` within each model.  
   - Keeps only those with a price spread above a threshold (e.g. 10,000), highlighting models that appear in both budget and premium trims.

4. **Compares price and mileage to global averages**  
   - Computes the overall average selling price and mileage.  
   - Flags cars that are **above-average price** but **below-average mileage**, which may represent poor value for money.

5. **Tracks cumulative selling price over time**  
   - Uses window functions like `SUM(selling_price) OVER (PARTITION BY Name ORDER BY year)` to compute cumulative revenue per model.  
   - Shows how the total market value for each model builds up across years.

6. **Searches for models priced near the overall average**  
   - Finds records where the selling price is within ±10% of the global average.  
   - Useful for identifying “typical” mid-range cars.

7. **Computes an exponential moving average (EMA)**  
   - Applies an EMA with a smoothing factor (for example, 0.2) per model, using window functions.  
   - Smooths out noise in the selling price trend and gives a more stable view of value over time.

8. **Detects year-on-year price drops**  
   - Uses `LAG(selling_price) OVER (PARTITION BY Name ORDER BY year)` to compare each year’s price to the previous year’s.  
   - Flags models where prices are falling, which might indicate waning demand or oversupply.

9. **Finds high-mileage workhorses per transmission type**  
   - Groups by `Name` and `transmission` and sums `km_driven`.  
   - Identifies models with the highest total mileage under manual vs automatic, i.e. which cars people really use.

10. **Ranks top models and tracks prices by year**  
    - Uses `RANK()` to find the top 3 models by overall selling price.  
    - Then calculates average yearly prices for those top models to see how their market value evolves.

---

## SQL Concepts Demonstrated

This project is a compact showcase of analytical SQL on a single but rich table:

- **Filtering and grouping** with `WHERE`, `GROUP BY`, and `HAVING`.  
- **Aggregations**: `AVG`, `SUM`, `MIN`, `MAX`.  
- **Window functions**:
  - `SUM() OVER` for cumulative totals  
  - `LAG()` for comparing current vs previous row  
  - `RANK()` to pick top models  
- Simple **business logic** built directly into queries, such as:
  - thresholds (price spread > 10,000)  
  - “near-average” conditions (within ±10%)  
  - segmentation by fuel, transmission, seats, and owner type.

Even though there’s no separate application layer here, this is effectively an end-to-end analytical slice: starting from a raw CSV of Indian car listings, loading it into a database table, and using SQL to answer targeted questions about pricing trends and vehicle behaviour.

---

## How to Run

1. Create a database (e.g. `cars_info`) in your SQL environment (Db2, PostgreSQL, etc.).  
2. Import the CSV file `Exploring Trends in the Automotive Industry.csv` into a table, for example `car_info`, with appropriate column types.  
3. Run the queries in [`Exploring Trends in the AutomotiveIndustry.sql`](Exploring%20Trends%20in%20the%20AutomotiveIndustry.sql) against that table.  

You can do this either in a database console or from a Jupyter notebook using `%load_ext sql` and a proper connection string.

---

## Author

**Yasir Savanur**  
LinkedIn: https://www.linkedin.com/in/yasir-savanur/
