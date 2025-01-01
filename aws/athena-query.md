# AWS Athena Query

AWS Athena is an interactive query service provided by Amazon Web Services that enables you to analyze data stored in Amazon S3 using standard SQL. The query language and DSL (Domain-Specific Language) used in AWS Athena is based on **Presto**, an open-source distributed SQL query engine.

### Overview of AWS Athena Query Language and DSL

#### 1. **SQL-Based Queries**

Athena uses a SQL dialect that is based on Presto's syntax, which is ANSI SQL compliant. It supports common SQL operations and functions for querying structured, semi-structured, and unstructured data.

##### Key Features of Athena's Query Language:

- **Standard SQL Support**: Includes SELECT, WHERE, GROUP BY, ORDER BY, JOIN, UNION, and more.
- **Support for Semi-Structured Data**: Queries can process JSON, Parquet, ORC, Avro, and other data formats.
- **Functions**: Provides support for a wide array of scalar, aggregate, and window functions.
- **Advanced Analytical Queries**: Includes support for nested queries, cross joins, and complex analytical expressions.
- **Extensibility**: Custom SerDes (Serializers/Deserializers) can be used for custom data formats.

---

#### 2. **Data Definition Language (DDL)**

Athena uses DDL for defining and managing the schema of the data stored in S3.

##### Common DDL Statements:

- **CREATE TABLE**: Defines a schema for the data in S3.
- **DROP TABLE**: Deletes a table schema but not the data in S3.
- **ALTER TABLE**: Modifies an existing table schema.
- **MSCK REPAIR TABLE**: Repairs a partitioned table to recognize new partitions.

Example:

```sql
CREATE EXTERNAL TABLE my_table (
    id INT,
    name STRING,
    created_at TIMESTAMP
)
STORED AS PARQUET
LOCATION 's3://my-bucket/my-folder/';
```

---

#### 3. **Data Manipulation Language (DML)**

Although Athena primarily serves as a read-only query service, it allows you to query and retrieve data but not to update, insert, or delete data directly. DML operations include SELECT statements to query data.

Example:

```sql
SELECT name, COUNT(*)
FROM my_table
WHERE created_at > '2023-01-01'
GROUP BY name
ORDER BY COUNT(*) DESC;
```

---

#### 4. **Functions and Expressions**

Athena provides a rich set of built-in functions for manipulating data.

##### Categories of Functions:

- **String Functions**: `SUBSTRING`, `UPPER`, `LOWER`, `REGEXP_EXTRACT`, etc.
- **Date and Time Functions**: `DATE_FORMAT`, `DATE_ADD`, `CURRENT_DATE`, etc.
- **Mathematical Functions**: `ABS`, `CEIL`, `FLOOR`, `RANDOM`, etc.
- **Aggregation Functions**: `SUM`, `AVG`, `COUNT`, `MIN`, `MAX`.
- **JSON Functions**: `JSON_EXTRACT`, `JSON_EXTRACT_SCALAR`.

Example:

```sql
SELECT JSON_EXTRACT(my_column, '$.key') AS key_value
FROM my_json_table;
```

**AWS Athena Functions Reference**

| Name           | Description                                     | Example                                                                 |
| -------------- | ----------------------------------------------- | ----------------------------------------------------------------------- |
| SUBSTRING      | Extracts a substring from a string.             | `SELECT SUBSTRING('Hello World', 1, 5); -- Returns 'Hello'`             |
| UPPER          | Converts a string to uppercase.                 | `SELECT UPPER('hello'); -- Returns 'HELLO'`                             |
| LOWER          | Converts a string to lowercase.                 | `SELECT LOWER('HELLO'); -- Returns 'hello'`                             |
| DATE_FORMAT    | Formats a date using a specified format.        | `SELECT DATE_FORMAT(DATE '2025-01-01', '%Y-%m'); -- Returns '2025-01'`  |
| DATE_ADD       | Adds a specified number of days to a date.      | `SELECT DATE_ADD('day', 5, DATE '2025-01-01'); -- Returns '2025-01-06'` |
| JSON_EXTRACT   | Extracts JSON data using a JSONPath expression. | `SELECT JSON_EXTRACT(json_column, '$.key');`                            |
| SUM            | Calculates the sum of a numeric column.         | `SELECT SUM(sales) FROM sales_table;`                                   |
| AVG            | Calculates the average of a numeric column.     | `SELECT AVG(price) FROM products;`                                      |
| REGEXP_EXTRACT | Extracts a match using a regular expression.    | `SELECT REGEXP_EXTRACT('abc123', '\\d+'); -- Returns '123'`             |
| RANDOM         | Generates a random number between 0 and 1.      | `SELECT RANDOM(); -- Returns a random float like 0.4324`                |

---

#### 5. **Partitioning and Bucketing**

Athena supports partitioning and bucketing to optimize query performance.

- **Partitioning**: Splits the data into partitions (e.g., by date).
- **Bucketing**: Further subdivides data within each partition.

DDL Example:

```sql
CREATE EXTERNAL TABLE partitioned_table (
    id INT,
    name STRING
)
PARTITIONED BY (year INT, month INT)
STORED AS PARQUET
LOCATION 's3://my-bucket/my-folder/';
```

---

#### 6. **Domain-Specific Language (DSL)**

Athena's DSL is the combination of its query language capabilities that allow you to interact with structured, semi-structured, and nested data. For example:

- Querying nested JSON using JSON path expressions.
- Aggregating data with complex filters.
- Joining data across multiple data sources.

Example of querying nested JSON:

```sql
SELECT
    JSON_EXTRACT_SCALAR(json_column, '$.user.name') AS user_name
FROM nested_json_table;
```

---

#### 7. **Key Integrations**

Athena integrates seamlessly with other AWS services:

- **AWS Glue**: For managing metadata catalogs and schema discovery.
- **Amazon S3**: For data storage.
- **AWS CloudTrail**: For auditing query activity.

---

### Use Cases

- Data exploration and analytics.
- Ad hoc querying for large datasets.
- Log analysis (e.g., analyzing AWS service logs in S3).
- ETL pipeline validation and schema verification.
