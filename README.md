# PublicAPI_ETL_SQLite_project
This project involves Environment setup and tool installation, Data extraction from a public API and CSV file with curl Data transformation using jq, awk, and sed, Data loading in an SQLite database with sqlite3.
# ETL Project

## 1. Setup

Install necessary tools:

```bash
sudo apt-get install curl jq awk sed sqlite3
Create a dedicated project directory:

bash
Copy code
mkdir ~/etl_project
cd ~/etl_project
## 2. EXTRACT

Fetch data from a public API and save it as a JSON file:

bash
Copy code
# Fetching data from a public API
curl -o data.json https://jsonplaceholder.typicode.com/users
Extract and transform specific fields from the JSON file:

bash
Copy code
# Parsing and extracting specific fields from JSON
jq '.data[] | {id, name, email}' data.json > transformed_data.json
3. TRANSFORM
Convert the JSON data to CSV:

bash
Copy code
jq -s -r '.[] | [.id, .name, .email] | @csv' transformed_data.json > transformed_data.csv
4. LOAD
Create a new SQLite database and table to hold the data:

bash
Copy code
# Creating a new SQLite database and table
sqlite3 etl_database.db "CREATE TABLE data (id INTEGER PRIMARY KEY, name TEXT, email REAL);"
Insert the transformed CSV data into the SQLite database:

bash
Copy code
# Inserting data into SQLite database
sqlite3 etl_database.db <<EOF
.mode csv
.import transformed_data.csv data
EOF
Verify that the data has been inserted correctly by querying the database:

bash
Copy code
# Querying the database
sqlite3 etl_database.db "SELECT * FROM data;"
This command retrieves all rows from the data table and displays them.

yaml
Copy code

---
