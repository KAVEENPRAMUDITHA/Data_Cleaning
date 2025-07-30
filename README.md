# Hotel Booking Data Cleaning Project

This project focuses on cleaning a hotel booking dataset. The primary goal is to preprocess the raw data to make it suitable for exploratory data analysis and machine learning modeling. The entire cleaning process is documented in the `notebooks/data_cleaning_process.ipynb` Jupyter Notebook.

## Project Structure

```
.
├── data/
│   ├── hotel_bookings.csv         
│   └── hotel_bookings_cleaned.csv 
├── notebooks/
│   └── data_cleaning_process.ipynb 
├── requirements.txt               
└── README.md                      
```

## Data Cleaning Process

The data cleaning process was divided into three main phases:

### Phase 1: Data Exploration and Assessment

1.  **Initial Data Inspection**: The dataset was loaded and examined to understand its structure, including the number of rows and columns, data types, and a statistical summary of the numerical features.
2.  **Missing Value Analysis**: A systematic check for missing values was performed. The columns `country`, `agent`, and `company` were identified as having missing data. A heatmap was used to visualize the missing data patterns.
3.  **Data Quality Assessment**:
    *   **Duplicate Records**: The dataset was checked for duplicate rows.
    *   **Outlier Detection**: Boxplots were used to identify potential outliers in numerical columns like `lead_time` and `adr`.
    *   **Inconsistency Checks**: Categorical columns were examined for inconsistent values, and the dataset was checked for illogical data entries (e.g., bookings with zero adults, children, and babies).

### Phase 2: Data Cleaning Implementation

1.  **Handling Missing Values**:
    *   `children`: Missing values were filled with `0`.
    *   `country`: Missing values were imputed using the mode (most frequent value).
    *   `agent` and `company`: Missing values were replaced with `0`, indicating no agent or company was involved. The data types for these columns were converted to `int`.
2.  **Duplicate Removal**: Exact duplicate rows were removed from the dataset.
3.  **Outlier Treatment**: Outliers in numerical columns were capped using the Interquartile Range (IQR) method to minimize their impact on analysis.
4.  **Fixing Inconsistencies**:
    *   Country codes were standardized to uppercase.
    *   The `reservation_status_date` was converted to a proper datetime format.
    *   Rows with illogical data, such as bookings with zero guests that resulted in a "Check-Out", were removed.

### Phase 3: Data Validation and Documentation

1.  **Validation Checks**: The cleaned dataset was validated to ensure that the cleaning steps were successful. This included verifying that there were no bookings with zero guests and that numerical values were within logical ranges.
2.  **Final Dataset**: The cleaned dataset was saved to `data/hotel_bookings_cleaned.csv`.

## Feature Engineering

To enhance the dataset for future analysis, several new features were created:

*   `total_nights`: The total number of nights stayed (weekend + week nights).
*   `total_guests`: The total number of guests (adults + children + babies).
*   `is_modified`: A binary flag indicating if the booking was modified.
*   `is_high_season`: A binary flag for bookings made in July or August.
*   `arrival_day_of_week`: The day of the week of the arrival date.
*   `booking_revenue`: An estimated revenue for each booking, calculated as `total_nights * adr`.

## Automated Cleaning Pipeline

For reusability, the key cleaning steps were encapsulated into functions to create an automated cleaning pipeline. This allows for the same cleaning process to be easily applied to new data.

## How to Run

1.  Clone the repository:
    ```bash
    git clone https://github.com/KAVEENPRAMUDITHA/Data_Cleaning.git
    ```
2.  Install the required dependencies:
    ```bash
    pip install -r requirements.txt
    ```
3.  Open and run the Jupyter Notebook `notebooks/data_cleaning_process.ipynb` to see the full data cleaning process.
