# Global Populations: Emerging Regions Project

Global Population Project  - Cleaned/Analyzed country growth rates in SQL to visualize a region of interest with an interactive Tableau dashboard. 

[Link to Interactive Dashboard](https://public.tableau.com/app/profile/nicolas.kiryczun/viz/EmergingRegions-TheArabianPeninsula/FinalDashboard)

![image](https://github.com/user-attachments/assets/2492b4ee-cd27-4e4b-9e31-1a18be3ec798)






## Skills Used

### SQL Skills 

Data cleaning, Data transformation, VIEW statements, JOIN clauses, Subqueries, Window functions

### Statistical Skills 

Descriptive statistics (Mean, Standard Deviation), Compound Growth Analysis

### Tableau Skills

Interactive filters, Geographic Visualization, Data Grouping, Time-Series Analysis

## About the Project

This project examines the global population dataset from the [ourworldindata.org](https://ourworldindata.org/) website and analyzes the data to identify regions with high population growth. It then delves deeper into one of these regions through an interactive Tableau dashboard.

The dataset includes four columns – Country, Year, Country Code, and Population, ranging from 10,000 BC to 2023. I uploaded the dataset onto DBeaver, creating a “clean” copy of the dataset for backup. I found multiple non-countries (continents, demographic measures, districts/capitals, etc.) and I removed those from the table. The dataset also had inconsistent population data for years earlier in human history (pre-1800s). I created three VIEW statements (for 1800, 1900, and 2023) to quickly retrieve population information from these specific years.

I then looked into the compound annual growth rate (CAGR) for different countries. The five countries with the lowest growth rates were all European countries, with the lowest being Ireland (0% CAGR). Looking at the countries with the highest CAGR since 2000, three of the five countries were African, and two were in the Middle East. One note with this query is that there was a population threshold of five million. Without this population threshold, four of the five countries were located in the Arabian Peninsula. This region had 3 - 4x higher growth than the average country. This discovery of an elevated growth rate in the Arabian Peninsula led to my decision to focus my Tableau dashboard on it.

I staged all relevant data into one master table for use in Tableau. Having all the data in one table allowed me to create interactive filters that would affect all parts of the dataset. My Tableau dashboard features a year slider, which allows users to modify the year range. Each visual is created to show a different aspect of this region, whether it be the total population gain, the difference in growth rates, or the difference in volatility, to give insight into its distinct growth pattern compared to other countries in this period.

## Links

[Tableau Dashboard](https://public.tableau.com/app/profile/nicolas.kiryczun/viz/EmergingRegions-TheArabianPeninsula/FinalDashboard)

[Original Dataset](https://ourworldindata.org/grapher/population?tab=table)

[Portfolio Website](https://nicolaskiryczun.github.io/)
