# Walmart Retail Intelligence DSS

## Project Title

Walmart Retail Intelligence DSS: OLAP, Fuzzy Logic, Machine Learning, and Gradio Deployment

## Project Overview

This project builds a retail Decision Support System using real Walmart sales data.

The system analyzes weekly sales performance across Walmart stores and departments.

It combines data warehousing, OLAP analysis, fuzzy logic, machine learning, and a simple Gradio deployment interface.

The main goal is to help management understand sales behavior and identify store-department combinations that need attention.

## Project Type

Data Science Project

Decision Support System

Retail Analytics

Business Intelligence

Machine Learning Deployment

## Dataset Source

The dataset used in this project is the Walmart Recruiting Store Sales Forecasting dataset from Kaggle.

Dataset link

https://www.kaggle.com/c/walmart-recruiting-store-sales-forecasting/data

## Why This Dataset Was Selected

This dataset was selected because it matches the retail Decision Support System objective.

It contains real historical Walmart sales records.

It includes multiple stores, departments, weekly sales, holidays, store information, promotions, and economic indicators.

The dataset is suitable for OLAP analysis because it contains measures and dimensions.

The dataset is suitable for fuzzy logic because it contains sales performance indicators, markdown values, holiday information, and local economic variables.

The dataset is suitable for machine learning because risk labels can be generated and predicted using engineered features.

## Business Problem

Retail managers need to understand why sales increase or decrease across stores and departments.

A full-store view is not enough.

The same store may have strong departments and weak departments at the same time.

The same department may perform well in one store and poorly in another store.

This project solves this problem by analyzing sales at the Store and Department level.

The DSS helps answer these business questions:

- Which stores generate the highest and lowest sales?
- Which departments generate the highest and lowest sales?
- How do sales change across months and quarters?
- Do holiday weeks affect sales?
- Do markdown promotions improve sales?
- Which store-department combinations have high sales risk?
- Can a machine learning model predict the risk level?
- Can the DSS be deployed in a simple interactive interface?

## Dataset Files

| File | Role | Description |
|---|---|---|
| train.csv | Fact table | Contains weekly sales records for each store and department |
| stores.csv | Dimension table | Contains store type and store size |
| features.csv | Dimension-like table | Contains external features such as markdowns, CPI, unemployment, fuel price, temperature, and holidays |

## Main Dataset Columns

| Column | Source File | Business Meaning |
|---|---|---|
| Store | train.csv, stores.csv, features.csv | Unique Walmart store number |
| Dept | train.csv | Department number inside a Walmart store |
| Date | train.csv, features.csv | Weekly sales date |
| Weekly_Sales | train.csv | Weekly sales amount for a store-department combination |
| IsHoliday | train.csv, features.csv | Shows whether the week includes a holiday |
| Type | stores.csv | Store category such as A, B, or C |
| Size | stores.csv | Physical size of the store |
| Temperature | features.csv | Local temperature around the store |
| Fuel_Price | features.csv | Local fuel price |
| MarkDown1 | features.csv | Promotion markdown value 1 |
| MarkDown2 | features.csv | Promotion markdown value 2 |
| MarkDown3 | features.csv | Promotion markdown value 3 |
| MarkDown4 | features.csv | Promotion markdown value 4 |
| MarkDown5 | features.csv | Promotion markdown value 5 |
| CPI | features.csv | Inflation and price level indicator |
| Unemployment | features.csv | Local economic pressure indicator |

## Data Warehouse Design

The project builds a merged analytical data warehouse view.

The original files are connected using shared business keys.

| Merge Step | Join Keys | Purpose |
|---|---|---|
| train.csv with stores.csv | Store | Add store type and store size to sales records |
| train.csv with features.csv | Store, Date, IsHoliday | Add economic, promotion, weather, and holiday features |

## Fact Table and Dimension Tables

After preparing the data warehouse, the final structure is organized into a fact table and dimension tables.

| Table | Type | Main Columns | Business Role |
|---|---|---|---|
| Fact_Sales | Fact table | Store, Dept, Date, Weekly_Sales, Total_MarkDown, Sales_Performance, Sales_Drop, Sales_Uplift | Stores measurable sales and DSS indicators |
| Dim_Store | Dimension table | Store, Type, Size | Describes store properties |
| Dim_Time | Dimension table | Date, Year, Quarter, Month, Month_Name, Week, Day, Holiday_Label | Supports time-based OLAP analysis |
| Dim_External | Dimension table | Store, Date, Temperature, Fuel_Price, CPI, Unemployment, MarkDown1 to MarkDown5 | Adds external business and economic context |

## Data Cleaning

The project applies the following cleaning steps:

- Load all dataset files using pandas
- Convert Date columns into datetime format
- Check missing values
- Fill missing MarkDown values with 0
- Handle missing CPI and Unemployment values
- Check duplicated records
- Prepare the final merged analysis table

## Feature Engineering

The project creates new features to support OLAP, DSS, fuzzy logic, and machine learning.

| Feature | Description |
|---|---|
| Year | Year extracted from Date |
| Month | Month number extracted from Date |
| Month_Name | Month name extracted from Date |
| Week | Week number extracted from Date |
| Day | Day of month extracted from Date |
| Quarter | Quarter extracted from Date |
| Total_MarkDown | Sum of MarkDown1 to MarkDown5 |
| Holiday_Label | Converts IsHoliday into Holiday or Non-Holiday |
| Sales_Performance | Weekly sales divided by average sales for the same store and department |
| Previous_Week_Sales | Previous weekly sales for the same store and department |
| Sales_Drop | Decrease in sales compared with the previous week |
| Sales_Uplift | Increase in sales compared with the previous week |
| Sales_Drop_Pct | Percentage decrease in sales |
| Markdown_Intensity | Total markdown compared with weekly sales |

## OLAP Analysis

The project applies the main OLAP operations required for retail decision support.

| OLAP Operation | Implementation | Business Meaning |
|---|---|---|
| Cube | Group sales by Store, Dept, Month, Quarter, Type, and Holiday_Label | View sales from multiple business dimensions |
| Roll-up | Aggregate sales from weekly level to monthly and quarterly level | Understand higher-level sales trends |
| Drill-down | Analyze sales from store level to department level and from month level to week level | Find the detailed source of sales changes |
| Slice | Filter one store or one holiday condition | Focus on one business segment |
| Dice | Filter by multiple stores, departments, and holiday condition | Analyze a specific business scenario |
| Pivot | Create a store-month sales matrix | Compare store sales across months |

## OLAP Visualizations

The project includes multiple charts to support DSS interpretation.

| Visualization | Purpose |
|---|---|
| Total Sales by Store | Identify best and weakest stores |
| Top Departments by Total Sales | Identify strongest revenue departments |
| Monthly Sales Trend | Analyze sales movement over time |
| Monthly Sales Share | Show each month contribution to total sales |
| Quarterly Sales Share | Compare sales contribution by quarter |
| Total Sales by Store Type | Compare sales between store categories |
| Holiday vs Non-Holiday Sales | Study holiday impact |
| Markdown vs Weekly Sales | Study relationship between promotions and sales |
| Sales Heatmap by Store and Month | Locate strong and weak store-month patterns |
| Sales Performance Distribution | Understand normal and abnormal sales performance |
| Fuzzy Risk Score Distribution | Show how risk levels are distributed |
| Risk Location by Store and Department | Identify exact high-risk combinations |

## DSS Interpretation

Each chart is followed by a DSS interpretation.

The interpretation explains:

- What the chart shows
- What the result means from a business view
- What management should do next

This turns the project from descriptive analysis into a real decision-support workflow.

## Fuzzy Logic DSS

The fuzzy logic system estimates sales risk for each Store and Department combination.

It does not evaluate the full store only.

It does not evaluate the department across all stores only.

It evaluates every store-department pair separately.

Examples:

| Store | Department | Meaning |
|---|---:|---|
| Store 1 | Dept 5 | Risk for Department 5 inside Store 1 |
| Store 1 | Dept 10 | Risk for Department 10 inside Store 1 |
| Store 2 | Dept 5 | Risk for Department 5 inside Store 2 |
| Store 20 | Dept 92 | Risk for Department 92 inside Store 20 |
| Store 33 | Dept 45 | Risk for Department 45 inside Store 33 |

## Fuzzy Logic Inputs

| Input | Business Meaning |
|---|---|
| Sales_Performance | Shows whether sales are below or above the normal level |
| Sales_Drop_Pct | Shows how much sales dropped compared with the previous week |
| Markdown_Intensity | Shows how strong the promotion pressure is compared with sales |
| Unemployment | Shows local economic pressure |
| IsHoliday | Shows whether the week is a holiday period |

## Fuzzy Logic Output

| Output | Description |
|---|---|
| Fuzzy_Risk_Score | Final risk score from 0 to 100 |
| Risk_Label | Low, Medium, or High |
| DSS_Recommendation | Business recommendation based on the risk level |

## Fuzzy Risk Score Mapping

| Fuzzy Risk Score | Risk Level | DSS Meaning | DSS Recommendation |
|---:|---|---|---|
| 0 to less than 35 | Low | Sales risk is low | Maintain current plan and monitor normally |
| 35 to less than 65 | Medium | Sales need attention | Monitor department performance and review promotions |
| 65 to 100 | High | Sales risk is high | Take management action by reviewing stock, pricing, and markdown strategy |

## Fuzzy Rules

The fuzzy DSS uses business rules to estimate risk.

| Rule | Business Meaning |
|---|---|
| If sales performance is low and sales drop is high, then risk is high | Weak sales with strong decline requires action |
| If sales performance is low and markdown intensity is high, then risk is high | High promotions without strong sales response is risky |
| If sales performance is high and sales drop is low, then risk is low | Strong and stable sales are safe |
| If unemployment is high and sales performance is low, then risk is high | Economic pressure may reduce customer spending |
| If holiday week is true and sales performance is low, then risk is high | Weak holiday performance needs management review |
| If sales performance is medium and sales drop is medium, then risk is medium | Moderate performance needs monitoring |
| If markdown intensity is low and sales performance is high, then risk is low | Good sales with low promotion pressure is healthy |
| If sales drop is high and markdown intensity is medium, then risk is high | Sales are falling despite promotion activity |

## Fuzzy DSS Business Value

The fuzzy DSS helps management find where risk appears exactly.

It can identify the exact department inside the exact store that needs attention.

This is more useful than only saying that a whole store is weak.

A store can have safe departments and risky departments at the same time.

The DSS supports targeted action.

## Machine Learning Model

A Random Forest classifier is trained to predict the fuzzy risk label.

The model uses engineered features from the retail data.

The target variable is the fuzzy risk label.

| Model | Purpose |
|---|---|
| Random Forest Classifier | Predict Low, Medium, or High risk |

## Machine Learning Role

The machine learning model works as a fast prediction layer.

The fuzzy logic system works as the explainable decision layer.

This means the model predicts quickly, while fuzzy logic explains why the decision was made.

## Model Evaluation

The model is evaluated using:

- Accuracy
- Classification report
- Confusion matrix
- Feature importance

The evaluation helps check whether the model can learn the DSS risk pattern.

## Gradio Deployment

The project includes a simple Gradio interface.

The interface simulates a deployment layer for the DSS.

The user can enter business values such as:

- Store
- Department
- Store type
- Store size
- Weekly sales
- Sales performance
- Sales drop
- Markdown intensity
- Unemployment
- CPI
- Holiday status

The application returns:

- Random Forest predicted risk label
- Fuzzy risk score
- Fuzzy risk level
- DSS recommendation

## Final DSS Summary

The final summary table includes the most important findings from the full analysis.

| Finding | Meaning |
|---|---|
| Highest total sales store | Store with the strongest total sales |
| Lowest total sales store | Store with the weakest total sales |
| Highest total sales department | Department with the strongest total sales |
| Lowest total sales department | Department with the weakest total sales |
| Number of high-risk store-department combinations | Exact number of combinations that need management action |
| Random Forest test accuracy | Performance of the prediction model |
| Markdown-sales correlation | Relationship between promotions and sales |
| Unemployment-sales correlation | Relationship between local economic pressure and sales |

## Final Recommendations

| Recommendation | Business Reason |
|---|---|
| Prioritize high-risk store-department combinations from the fuzzy DSS table | They combine weak performance, sales drops, promotion pressure, and economic risk |
| Review departments with high markdown intensity and weak uplift | High discount spending without sales response can reduce profitability |
| Plan holiday inventory using monthly and weekly sales trends | Seasonal peaks and drops are visible in OLAP charts |
| Compare stores within the same store type before judging performance | Different store types have different sales capacity |
| Use the Random Forest model as a quick deployment layer | The model gives fast predictions |
| Keep fuzzy logic as the explainable decision layer | Fuzzy rules explain the business reason behind the risk decision |
| Create weekly monitoring for Sales_Performance, Sales_Drop_Pct, and Markdown_Intensity | These features directly drive DSS risk recommendations |

## Project Workflow

1. Import required libraries
2. Load Walmart dataset files
3. Understand tables and business meaning
4. Clean missing values
5. Convert Date columns into datetime format
6. Merge data into one data warehouse view
7. Create time features
8. Create DSS features
9. Display fact table and dimension tables
10. Apply OLAP operations
11. Create visualizations
12. Add DSS interpretations
13. Build fuzzy logic risk system
14. Apply fuzzy logic to store-department combinations
15. Train Random Forest model
16. Evaluate model performance
17. Build Gradio deployment interface
18. Generate final DSS summary
19. Generate final recommendations

## Technologies Used

| Technology | Purpose |
|---|---|
| Python | Main programming language |
| Pandas | Data loading, cleaning, merging, and analysis |
| NumPy | Numerical processing |
| Matplotlib | Data visualization |
| Seaborn | Statistical visualizations |
| Scikit-learn | Machine learning model training and evaluation |
| Gradio | Simple deployment interface |
| Jupyter Notebook | Full project development environment |

## How to Run the Project

Step 1

Download the dataset from Kaggle.

https://www.kaggle.com/c/walmart-recruiting-store-sales-forecasting/data

Step 2

Place the dataset files in the project folder.

Expected files:

| File |
|---|
| train.csv |
| stores.csv |
| features.csv |

Step 3

Open the notebook.

| Notebook |
|---|
| main.ipynb |

Step 4

Run all cells from top to bottom.

Step 5

Run the Gradio cell at the end to launch the DSS interface.

## Suggested Repository Structure

| File or Folder | Description |
|---|---|
| main.ipynb | Main Jupyter Notebook |
| README.md | Project documentation |
| train.csv | Walmart sales fact table |
| stores.csv | Walmart store dimension table |
| features.csv | Walmart external features table |
| images | Optional folder for charts and screenshots |

## Key Skills Demonstrated

- Data cleaning
- Data warehouse design
- Business intelligence
- OLAP analysis
- Feature engineering
- Retail analytics
- DSS interpretation
- Fuzzy logic
- Machine learning classification
- Model evaluation
- Explainable decision support
- Gradio deployment
- Data storytelling

## Business Value

This project shows how data science can support retail management decisions.

The system helps management:

- Identify strong stores
- Identify weak stores
- Detect strong departments
- Detect weak departments
- Understand seasonal sales behavior
- Analyze holiday impact
- Review markdown effectiveness
- Locate high-risk store-department combinations
- Use fuzzy rules for explainable risk decisions
- Use machine learning for quick predictions
- Use Gradio for simple deployment

## Conclusion

This project demonstrates a complete retail Decision Support System using real Walmart sales data.

The system starts with raw data and ends with business recommendations.

It combines OLAP, visual analytics, fuzzy logic, machine learning, and deployment.

The final output helps management understand sales performance, locate risk, and take practical actions.
