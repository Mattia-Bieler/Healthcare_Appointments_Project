# Healthcare Appointments Project
## Project Background
This project focuses on analysing service capacity utilisation within the National Health Service (NHS) to identify patterns of overutilisation and assess the financial implications of missed General Practitioner (GP) appointments. By quantifying the costs of non-attendance and examining the contributing factors, the project aims to uncover key insights into the operational challenges faced by the NHS. The analysis is particularly important given the breakdown in coordination between regions and departments, which has led to a fragmented and incomplete view of service availability and appointment attendance.

The project will provide a data-driven overview of current appointment trends in relation to service capacity utilisation, highlighting areas of inefficiency and financial strain. Based on these findings, actionable strategies will be proposed to optimise resource allocation, improve service delivery, and reduce financial losses from unattended GP appointments. The goal is to create an evidence-based framework that enhances operational decision-making and helps the NHS improve patient care while minimising costs.

## Analytical Approach
#### Jupyter Notebook Preparation__
To enhance efficiency, several functions were implemented at the start of the Jupyter notebook to automate key tasks, minimise code repetition, and ensure consistency. These functions include:
| **Function Name**                   | **Description** |
|-------------------------------------|-----------------|
| **`find_unique_values`**            | Identifies and prints unique values in a specified column. |
| **`process_date_column`**           | Converts a column to datetime format, finds the first and last dates, calculates the total number of days between them, and checks for invalid conversions. |
| **`count_show_duplicates`**         | Identifies and counts duplicate rows, returning the first 20 duplicates if present. |
| **`remove_duplicates`**             | Removes duplicate rows and returns the cleaned DataFrame. |
| **`check_negative_value_count`**    | Checks for negative values in a specified column. |
| **`calculate_total`**               | Calculates the sum of a specified numeric column. |
| **`check_one_to_one_relationship`** | Verifies whether two columns have a one-to-one relationship by checking unique value mappings. |
| **`check_column_values_match`**     | Ensures all values in a specified column of one DataFrame exist in another DataFrame and vice versa. |


__Initial Analysis__ <br>
After defining the key functions, I imported the datasets as separate DataFrames and applied the functions for cleaning and validation. Furthermore, I used the info function to summarise their structure, including non-null counts, data types, and memory usage. The DataFrames were named:
| **DataFrame Name** | **Source File**                  |
|--------------------|----------------------------------|
| **`ad`**           | **`actual_duration.csv`**        |
| **`ar`**           | **`appointments_regional.csv`**  |
| **`nc`**           | **`national_categories.csv`**    |

For the ad DataFrame, region_ons_code values were replaced with corresponding region names, stored in a new column region_name, and the original column was removed to maintain a clean dataset. Then, the find_unique_values function was applied to verify unique values in region_name. Furthermore, an NHS colour palette was defined, mapping each region_name to an official colour code for visualisations. Finally, a region_metadata DataFrame was created, ensuring each sub_icb_location_code appeared only once by selecting relevant location columns.

For the ar DataFrame, the find_unique_values function identified unnecessary spaces in time_between_book_and_appointment, which were removed. Values were then standardised for clarity: “More than 28 Days” was changed to “Over 28 Days”, and “Unknown / Data Quality” was replaced with “Unknown”. Furthermore, duplicates were identified. However, as the data comes from multiple systems across different sub-ICBs, the lack of a sub-ICB classification codes column, it is unclear whether they are actual duplicates or valid entries. Therefore, the potential duplicates were not removed.

For the nc DataFrame, a temporary DataFrame (temp_nc) was created, containing only appointment_date and appointment_month. appointment_date values were reformatted to match the '%Y-%m' format of appointment_month. A match column was added to verify whether the two values were identical, and discrepancies were identified by counting rows with “False” in the match column. The results confirmed that all appointment_date values correctly matched appointment_month, ensuring data integrity before further analysis.

After data cleaning and validation, I conducted an initial analysis, with the summarise_appointments function playing a key role. This function aggregates appointment counts by a specified category (e.g. appointment_status or time_between_book_and_appointment) and calculates each category's percentage share. It groups the data, sums count_of_appointments, and generates a sorted DataFrame, providing clear insights into appointment distributions. Another important function was the check_required_columns function. This function verifies whether a DataFrame contains a specified set of required columns by identifying which columns are present and which are missing, then printing the results. Using the check_required_columns function and the region_metadata DataFrame, I was able to add a region_name column to the nc DataFrame. After this initial analysis, I moved on to the main analysis to address the objectives of the project. For this, visualisations played a central role.

## Visualisations
At the beginning of the Jupyter notebook, custom configurations were set up to ensure consistent and accessible visualisations, prioritising clarity by making plot legends, labels, and titles bold, as well as setting their font size. The NHS blue (#005EB8) and NHS aqua blue (#00A9CE) were integrated into Seaborn’s colourblind-friendly palette to reflect NHS branding, positioned as the first and last colours in the palette, respectively. Furthermore, to enhance visual clarity, the 5th and 6th colours were swapped, and the 7th and 9th colours were removed, as they closely resembled other colours in the palette, which could lead to confusion.

The decision to use barplots and lineplots was driven by their simplicity and ease of interpretation. Both graph types are widely recognised and effective for comparing different categories. Additionally, lineplots offer the advantage of illustrating how these categories change over time, providing a clear view of trends and patterns. Furthermore, date tick labels were formatted as '%b %y' (e.g. Jan 20) for monthly appointment groupings to keep labels concise and avoid clutter, while '%d %b %y' (e.g. 01 Jan 20) was used for daily appointment plots to clearly illustrate the start of each month. To optimise the Jupyter notebook and reduce repetitive code, several functions were defined to generate the plots. 

## Insights and Recommendations
#### Service Capacity Utilisation
![ad Monthly Threshold](https://github.com/user-attachments/assets/665791ce-cc40-4411-936f-2a130e6c30d4)
![ar Monthly Threshold](https://github.com/user-attachments/assets/46131477-e5c5-4d06-8e78-172f01246c48)
![nc Monthly Threshold](https://github.com/user-attachments/assets/cc323fdc-510c-4598-8f44-7d4b5eedeb7d)
To assess NHS service capacity utilisation, I analysed monthly and daily appointment trends across the ad, ar, and nc DataFrames. A monthly threshold was established by multiplying the daily threshold (1,200,000) by the number of days in each month. Across all three DataFrames, monthly appointments remained below this threshold. The highest monthly utilisation was recorded in March 2022 for the ad DataFrame (73.04%) and in November 2021 for both the ar and nc DataFrames (84.46%). Visualising monthly appointments on separate line plots showed that the nc DataFrame aligned with the ar DataFrame from August 2021 to June 2022. Merging both DataFrames and comparing total monthly appointments confirmed no discrepancies during this period. However, a further analysis should be done to confirm whether the nc DataFrame is a sub-set of the ar DataFrame.

![ad Daily Threshold](https://github.com/user-attachments/assets/9cae7945-3340-4d57-a878-485ca987a2b7)
![nc Daily Threshold](https://github.com/user-attachments/assets/f33a3611-72ab-4561-bd4c-96fbc778104f)
A daily analysis provided deeper insights, revealing threshold exceedances despite monthly stability. In the ad DataFrame, 48 of 212 days (22.64%) surpassed 1,200,000 appointments, exclusively on Mondays (26) and Tuesdays (22), indicating operational strain at the start of the week. The nc DataFrame showed more frequent exceedances (175 of 334 days, 52.40%) with a broader weekday distribution: Mondays and Tuesdays (43 each), Wednesdays (36), Thursdays (34), and Fridays (19), suggesting sustained weekday pressure.
