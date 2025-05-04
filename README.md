# TASK 3 Database-Migration

COMPANY NAME: CODTECH IT SOLUTIONS

NAME: SOWMYA SRI K

INTERN ID: CT04DA63

DOMAIN NAME: SQL

DURATION: "4 Weeks"

MENTOR NAME: NEELA SANTOSH

---In this we learn, how to migrate data between two databases (Eg: From My SQL to PostgreSql)

---This project demonstrates how to migrate a sample relational database from MySQL to PostgreSQL, ensuring structural and data integrity throughout the process. 
---It includes database schemas, sample data, migration scripts, and data validation steps.

PROJECT STRUCTURE:
mysql-to-postgresql-migration/
│
├── mysql/
│   ├── schema.sql
│   └── sample_data.sql
│
├── postgresql/
│   └── schema.sql
│
├── migration/
│   └── migrate.py
│
├── validation/
│   └── validate_data.py
│
├── README.md
└── requirements.txt

* SAMPLE DATA:
  MYSQL Schema and Data

  CREATE TABLE Employee (
  Employee_id INT Primary Key,
  first_name VARCHAR(50),
  last_name VARCHAR(50),
  Start_date DATE,
  salary VARCHAR(50)
  );

  INSERT INTO Employee (Employee_id, first_name, last_name, Start_date, salary) VALUES
  (1, 'Anusha', 'Lara', '2022-04-15', '250000'),
  (2, 'Sowmya', 'Kakara', '2023-02-13', '400000'),
  (3, 'Khyati', 'Sen', '2024-05-10', '300000'),
  (4, 'Karan', 'Dhoval', '2024-11-25','4500000');

  PostgreSQL Schema:

  CREATE TABLE Employee (
  employee_id INT PRIMARY KEY,
  first_name VARCHAR(50),
  last_name VARCHAR(50),
  start_date DATE,
  salary VARCHAR(50)
  );

* Migration Script (pgLoader)
scripts/migrate_with_pgloader.sh

pgloader mysql://user:password@localhost/sample_db \
         postgresql://user:password@localhost/new_db

Replace user, password, and database names as needed.

* Data Integrity Validation
a) Record Count Comparison
validation/validation_queries.sql

-- MySQL
SELECT COUNT(*) FROM Employee;

-- PostgreSQL
SELECT COUNT(*) FROM Employee;

b) Data Hash/Checksum Comparison
-- MySQL
SELECT MD5(CONCAT_WS('#', employee_id, first_name, last_name, start_date, salary)) AS rowhash FROM Employee ORDER BY employee_id;

-- PostgreSQL
SELECT MD5(CONCAT_WS('#', employee_id, first_name, last_name, start_date, salary)) AS rowhash FROM Employee ORDER BY employee_id;

c) Python Validation Script
scripts/validate_data.py

import pymysql
import psycopg2

# MySQL connection
mysql_conn = pymysql.connect(host='localhost', user='user', password='password', db='sample_db')
mysql_cur = mysql_conn.cursor()
mysql_cur.execute("SELECT * FROM Employee ORDER BY employee_id")
mysql_data = mysql_cur.fetchall()

# PostgreSQL connection
pg_conn = psycopg2.connect(host='localhost', dbname='new_db', user='user', password='password')
pg_cur = pg_conn.cursor()
pg_cur.execute("SELECT * FROM Employee ORDER BY employee_id")
pg_data = pg_cur.fetchall()

assert mysql_data == pg_data, "Data mismatch between MySQL and PostgreSQL!"

print("Validation successful: Data matches in both databases.")

mysql_conn.close()
pg_conn.close()

* README.md Example
# MySQL to PostgreSQL Migration Example

This project demonstrates how to migrate a sample employee table from MySQL to PostgreSQL and validate data integrity.

## Steps

1. **Set up MySQL and PostgreSQL databases**
   - Use `mysql/schema.sql` and `mysql/sample_data.sql` for MySQL.
   - Use `postgres/schema.sql` for PostgreSQL.

2. **Run Migration**
   - Use `scripts/migrate_with_pgloader.sh` to migrate data.

3. **Validate Data**
   - Run the SQL queries in `validation/validation_queries.sql`.
   - Or use `scripts/validate_data.py` to automate validation.

## Sample Data

| employee_id | first_name | last_name   | start_date  | salary  |
|-------------|------------|--------------|-------------|----------
| 1           | Anusha     |    Lara      | 2022-04-15  | 250000  |
| 2           | Sowmya     | Kakara       | 2023-02-13  | 400000  |
| 3           | Khyathi    |Sen           | 2024-05-10  | 300000  |
| 4           | Karan      | Dhoval       | 2024-11-25  | 450000  |




  
  
