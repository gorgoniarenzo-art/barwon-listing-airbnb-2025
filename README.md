# Barwon Airbnb Insights: Location, Privacy and Profit

## Introduction

This project utilizes a full-year dataset (2024-2025 period) to decode the geographic and property-type trends of the Barwon region. By analyzing the annual distribution of rental performance, this project decodes the 'Location Drivers' that dictate pricing power. The resulting insights empower hosts and investors to optimize revenue by identifying high-value geographic trends and property-type preferences.

## Project Overview

In a coastal market like Barwon, location is more than just a coordinate—it is the primary driver of value. This analysis evaluates 9 neighborhoods to distinguish between "Saturated Markets" (high competition) and "Luxury Niches" (low supply/high value), providing a data-driven roadmap for maximizing Average Daily Rate (ADR).

## Tools Used

* **Data Cleaning:** SQL (BigQuery)
* **Analysis:** SQL (BigQuery)
* **Visualization:** Tableau
* **Documentation:** GitHub

## Data Source

[Link to Inside Airbnb Barwon Dataset](https://insideairbnb.com/get-the-data/)

## The Data Analysis Process

### 1. Ask

**Business Task:** 

Identify how neighborhood and room type influence profit to determine the most lucrative investment combinations in the Barwon region.

**Key Stakeholders:** 

Individual Hosts, Real Estate Investors, and Local Tourism Boards.

### 2. Prepare

**Data Source:** 

4 quarterly CSV snapshots (Dec 2024 – Sept 2025) from Inside Airbnb.

**Organization:** 

Data is organized by unique listing ID, neighborhood, room type, and standardized geographic coordinates.

**ROCCC Check:** 

Data is Reliable, Original, Comprehensive, Current (Sept 2025), and Cited.

**Known Limitations:** 

Analysis excludes <0.5% of records due to formatting anomalies; accounts for advertised rates rather than actualized transaction data.

### 3. Process

**Data Consolidation:** 

I merged four quarterly datasets using `UNION ALL` and implemented `SAFE_CAST` to standardize inconsistent data types (Price, Latitude, Longitude) into `FLOAT64` format.

**Coordinate Correction:** 

Due to CSV parsing errors, some coordinates were transposed. I used `CASE` statements to detect and flip swapped latitude/longitude values, ensuring geographic accuracy for Australia’s southern hemisphere.

**Market Trimming:** 

To eliminate "Price Noise," I established a range of **$40 to $900 AUD**. This excluded placeholder listings ($0–$39) and extreme outliers above the 95th percentile, focusing the analysis on the realistic investable market.

***Sample Cleaning and Transformation Query***

```sql

-- Consolidating data with standardization and temporal labeling
CREATE TABLE `barwon_airbnb.barwon_full_year` AS
SELECT id, neighbourhood, room_type, 
       SAFE_CAST(price AS FLOAT64) AS price,
       SAFE_CAST(latitude AS FLOAT64) AS latitude,
       'Q1' AS quarter
FROM `barwon_airbnb.barwon_q1`
UNION ALL 
-- ...continued for Q2, Q3, and Q4

```

### 4. Analyze

**The "Coastal Premium" Effect:** 

Beachfront neighborhoods, particularly in the Surf Coast, command significantly higher ADRs regardless of listing density, proving that location desirability outweighs competitive volume.

**Entire Home Dominance:** 

Entire home/apt listings dominate with a 91.92% market share, signaling that Barwon travelers prioritize privacy and space over shared living options.

**Market Segmentation:** 

By comparing price against density, I identified "Luxury Niches"—neighborhoods with low supply but high ADR—representing the most underserved investment zones in the region.

Queenscliffe has an average price of $392.96 but a density of only 854, making it a textbook "Luxury Niche".

### 5. Share

[View my Tableau Public Viz](https://public.tableau.com/app/profile/arrenz.gorgonia/viz/BarwonAirbnbInsightsLocationPrivacyandProfit/Dashboard1)

### 6. Act

**Top 3 Recommendations:**

1. **Prioritize Location Quality:**
  
Target "Low Density + High Price" coastal pockets. These underserved areas offer the highest ROI potential with minimal competitive friction.

2. **Optimize for Entire Units:**

In this market, the "Privacy Bonus" is substantial. Converting private rooms to entire guest suites can significantly increase pricing power.

3. **Differentiate in Saturated Zones:**
  
If operating in high-density urban areas, invest in unique amenities (e.g., hot tubs or pet-friendly features) to justify a price floor above the neighborhood average.

