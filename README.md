# Australian Retail Store Data Scraper

## Overview

This project is a Python-based data engineering pipeline that scrapes, cleans, and stores location information for two Australian retail chains: **BIG W** and **Choice The Discount Store**. The script fetches data from a public API and a website, processes the information into a structured format, geocodes addresses to obtain coordinates, and then loads the final dataset into a MySQL database and a CSV file.

### Key Features
*   **Multi-Source Scraping**: Extracts data from a JSON API (BIG W) and through HTML parsing with BeautifulSoup (Choice).
*   **Data Cleaning & Standardization**: Cleans and formats inconsistent data fields like phone numbers, addresses, and store hours.
*   **Geocoding**: Uses `geopy` to convert physical addresses into latitude and longitude coordinates.
*   **Database Integration**: Loads the cleaned data into a MySQL database using `SQLAlchemy` and `mysql-connector-python`.
*   **Data Export**: Merges all processed data into a final, comprehensive CSV file for analysis.

## Workflow

The script executes the following steps:

1.  **BIG W Data Extraction**:
    *   Makes a GET request to the BIG W public API to retrieve a JSON object containing store data.
    *   Parses the JSON data and loads it into a pandas DataFrame.
    *   Cleans and restructures the DataFrame to create standardized columns for address, location, and contact information.

2.  **Choice The Discount Store Data Extraction**:
    *   Scrapes the store finder webpage using `requests` and `BeautifulSoup`.
    *   Parses the HTML to extract store names, addresses, and phone numbers.
    *   Handles missing or irregular data points by filling them with 'N/A'.
    *   Uses the `geopy` library to geocode store addresses, obtaining latitude and longitude.

3.  **Data Consolidation**:
    *   Combines the two separate DataFrames (for BIG W and Choice) into a single, unified DataFrame.
    *   Processes and structures detailed information, such as opening hours and available services, for each store.

4.  **Database Storage**:
    *   Connects to a local MySQL database.
    *   Creates and populates two tables:
        *   `store_info`: Contains primary location data (address, coordinates, phone number).
        *   `store_details`: Contains secondary data (opening hours, services).

5.  **Final Output**:
    *   Merges the `store_info` and `store_details` data into a final DataFrame.
    *   Exports this final, comprehensive dataset to a CSV file named `brand_store_info.csv`.

## Prerequisites

Before running the script, ensure you have a local MySQL server running. You will also need to install the required Python libraries.

### Database Setup

1.  Make sure your MySQL server is active.
2.  Create a database. The script is configured to use a database named `reomnify_data`.
    ```sql
    CREATE DATABASE reomnify_data;
    ```
3.  Update the database connection credentials in the script to match your local setup:
    ```python
    host = 'localhost'
    user = 'your_mysql_username'
    password = 'your_mysql_password'
    database = 'reomnify_data'
    ```

### Requirements

This project relies on several Python libraries. You can install them using pip:

```bash
pip install requests pandas beautifulsoup4 geopy mysql-connector-python sqlalchemy
```

## How to Run

1.  **Clone or download the project files.**
2.  **Set up your MySQL database** and update the credentials in the script as described above.
3.  **Install the required libraries** using the command in the requirements section.
4.  **Execute the script** from your terminal:
    ```bash
    python your_script_name.py
    ```

## Output

After the script runs successfully, you will have the following outputs:

### 1. CSV File

A file named `brand_store_info.csv` will be created in the same directory. It contains the fully merged and cleaned data with the following columns:

| ColumnName     | Description                                     |
| :------------- | :---------------------------------------------- |
| `brandName`    | The name of the retail brand (e.g., "BIG W").   |
| `storeName`    | The specific name of the store location.        |
| `phoneNumber`  | The contact phone number for the store.         |
| `street`       | The street address.                             |
| `suburb`       | The suburb of the store's location.             |
| `state`        | The state (e.g., NSW, QLD).                     |
| `country`      | The country (Australia).                        |
| `postcode`     | The postal code.                                |
| `latitude`     | The geographic latitude.                        |
| `longitude`    | The geographic longitude.                       |
| `openingHours` | A list of the store's opening hours for the week. |
| `services`     | A list of services available at the store.      |

### 2. Database Tables

Two tables will be created and populated in your `reomnify_data` MySQL database:
*   **`store_info`**: Contains the primary location and contact details.
*   **`store_details`**: Contains the opening hours and services information, linked by brand and store name.
