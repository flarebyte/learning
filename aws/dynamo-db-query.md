# Amazon DynamoDB query

Amazon DynamoDB is a fully managed NoSQL database service designed for fast and flexible performance, suitable for a variety of applications. A **DynamoDB query** is a focused operation that retrieves items from a table based on the **primary key** or **secondary indexes**. Queries are efficient as they use the partition key and, optionally, a sort key to filter results. The operation allows further filtering using expressions to narrow down the dataset, but these filters are applied after the initial query execution. Queries are ideal for scenarios where you need to retrieve data that shares the same partition key or falls within a range of sort key values. The results can be sorted in ascending or descending order and include options for pagination to handle large datasets. DynamoDB's query capability is cost-effective as it retrieves only the necessary data and avoids full table scans, adhering to provisioned throughput limits for optimal performance.

In Amazon DynamoDB, **filtering using expressions** is a mechanism to refine query or scan results by applying conditions to attributes. While both **Query** and **Scan** operations can use filter expressions, the key difference is that filters are applied after the data is retrieved from the database, meaning they do not reduce the read capacity units consumed.

## Key Features of Filtering with Expressions:

1. **Syntax and Structure**:

   - Filter expressions are written using DynamoDB's expression syntax, which combines **attribute names**, **comparison operators** (e.g., `=`, `<`, `>`, `<>`), **logical operators** (e.g., `AND`, `OR`, `NOT`), and **functions** (e.g., `begins_with`, `contains`).
   - They use **placeholders** for attribute names (e.g., `#attr`) and values (e.g., `:value`) to prevent conflicts with reserved keywords.

2. **Examples of Filter Expressions**:

   - `#age > :min_age`: Filters items where the `age` attribute is greater than a specified value.
   - `begins_with(#name, :prefix)`: Matches items where the `name` attribute starts with a specific string.
   - `contains(#tags, :tag_value)`: Finds items where the `tags` attribute includes a specific value.

3. **Post-Query Filtering**:

   - In a **Query**, the filter expression is applied after retrieving items based on the partition key and optional sort key conditions.
   - For **Scan**, the filter is applied after scanning the entire table.

4. **Performance Consideration**:

   - Filter expressions do not optimize the retrieval process itself. For Queries, you should design the schema to limit retrieved items using keys rather than relying solely on filters.
   - Since filters are post-retrieval, they can lead to inefficiency if a large number of items must be scanned or queried before applying the filter.

5. **Use Cases**:
   - Narrowing results in a query by additional criteria, such as retrieving all orders for a user but only those exceeding a certain amount.
   - Refining scanned data in cases where you cannot structure the primary key or secondary indexes to meet all conditions upfront.

### Supported functions

Here is a table with all the **supported functions** in DynamoDB filter, condition, and update expressions, along with their descriptions and examples:

| **Function**               | **Description**                                                                   | **Example**                                                                                                   |
| -------------------------- | --------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| **`attribute_exists`**     | Checks if a specified attribute exists in the item.                               | `attribute_exists(#email)` <br> Filters items where the `email` attribute exists.                             |
| **`attribute_not_exists`** | Checks if a specified attribute does not exist in the item.                       | `attribute_not_exists(#deleted)` <br> Filters items where the `deleted` attribute is absent.                  |
| **`attribute_type`**       | Checks if an attribute is of a specific data type.                                | `attribute_type(#age, :type_number)` <br> Filters items where the `age` attribute is a number.                |
| **`begins_with`**          | Checks if a string attribute begins with a specific substring.                    | `begins_with(#name, :prefix)` <br> Filters items where the `name` attribute starts with "John".               |
| **`contains`**             | Checks if a string contains a substring or a set/array contains a specific value. | `contains(#tags, :value)` <br> Filters items where the `tags` attribute contains "urgent".                    |
| **`size`**                 | Returns the size of a string, set, or list attribute.                             | `size(#description) > :min_length` <br> Filters items where the `description` length is greater than a value. |
| **`starts_with`**          | Alias for `begins_with`. Checks if a string starts with a specific prefix.        | `starts_with(#title, :prefix)` <br> Filters items where `title` starts with "AWS".                            |
