# PublicAPI_ETL_SQLite_project

This project involves environment setup and tools installation, data extraction from a public API and CSV file with curl, data transformation using jq, awk, and sed and finally data loading in an SQLite database with sqlite3.
> ⚠️ **Note:** This project uses mock data from a public API (JSONPlaceholder) for simple demonstration purposes. No real or sensitive data is involved.

# Simple ETL (Beginner-Friendly)

## 1. Environment setup

Install necessary tools:

```
sudo apt-get install curl jq awk sed sqlite3
```
Create a dedicated project directory:

```
mkdir ~/etl_project
cd ~/etl_project
```

## 2. EXTRACT

Fetch data from a public API and save it as a JSON file:

```
# Fetching data from a public API
curl -o data.json https://jsonplaceholder.typicode.com/users
```

Extract and transform specific fields from the JSON file:

```
# Parsing and extracting specific fields from JSON
jq '.data[] | {id, name, email}' data.json > transformed_data.json
```

## 3. TRANSFORM
Convert the JSON data to CSV:

```
jq -s -r '.[] | [.id, .name, .email] | @csv' transformed_data.json > transformed_data.csv
```

## 4. LOAD
Create a new SQLite database and table to hold the data:

```
# Creating a new SQLite database and table
sqlite3 etl_database.db "CREATE TABLE data (id INTEGER PRIMARY KEY, name TEXT, email REAL);"
```

Insert the transformed CSV data into the SQLite database:

```
# Inserting data into SQLite database
sqlite3 etl_database.db <<EOF
.mode csv
.import transformed_data.csv data
EOF
```

### Finally verify that the data has been inserted correctly by querying the database:

```
# Querying the database
sqlite3 etl_database.db "SELECT * FROM data;"
```

This command retrieves all rows from the data table and displays them.
