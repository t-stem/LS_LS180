# LS180 Study Guide

## SQL

- SQL is a special-purpose language (as opposed to general-purpose languages like JavaScript, Python etc.)
- SQL is a declarative language, which expresses the desired outcome (what) but not the way to obtain that outcome (how)

### Data vs. Schema

- Schema: 
  * the structure of a database in terms of tables, columns, relationships and constraints
  * ultimately, the goal of the schema is to ensure data integrity
  * *organizes data so the database can model the problem domain clearly and efficiently*
  * *Types of schema:*
    - conceptual schema: high-level entities and relationships
    - logical schema: tables, columns, keys, relationships, constraints
    - physical schema: implementation details in the database system

- Data: 
  * the content of the database, i.e. the values that are stored in the columns of tables in the database

### Name and define the three sublanguages of SQL and be able to classify different statements by sublanguage.

- Data Definition Language
  * SQL language that defines the schema
  * Includes the following commands/keywords
    - CREATE database_object_name
      * used to create a new database_object that doesn't yet exist in the database
      * precedes TABLE, INDEX, SEQUENCE schemas
    - ALTER database_object_name
      * used to change properties of a database object that already exists in the database
      * precedes TABLE, COLUMN, INDEX, SEQUENCE objects
    - DROP
      * used to remove parts of the schema permanently
      * precedes TABLE, COLUMN, INDEX, SEQUENCE, CONSTRAINT
    - TABLE
      * structure in the database that may represent an entity
      * consists of columns and rows that contain data records
    - PRIMARY KEY
      * assigns a column that contains a unique identifier for every record
      * cannot be NULL
    - ADD
      * can be used to add columns to a table
      * used to add constraints to the schema
      * can follow ALTER TABLE table_name for table-level constraints
      * can follow ALTER COLUMN for column-level constraints
      * works with CONSTRAINT
    - CONSTRAINT constraint_name
      * adds a constraint to a part of the database (table or column)
      * follows ADD or DROP
      * works with UNIQUE, CHECK, FOREIGN KEY
    - UNIQUE (column)
      * checks whether all values in a column or in multiple columns are unique
      * follows CONSTRAINT
    - CHECK (condition)
      * Checks whether a condition is true
      * follows CONSTRAINT
    - FOREIGN KEY
      * establishes a foreign key that references values in another table
      * followed by REFERENCES to indicate where the foreign key points to (more later)
      * can be followed by ON DELETE CASCADE (more later)
      * follows CONSTRAINT
    - SET
      * used to set certain column properties
      * works with TYPE, NOT NULL, DEFAULT
    - *DEFAULT*
      * sets a default value that's used if no explicit value is included in a new record entry
      * used with SET 
    - TYPE
      * determines the type of data that is allowed to be stored in a column
      * Includes:
        - BOOLEAN: binary true or false values
        - INTEGER/INT: integer numbers
        - SERIAL: series of unique, increasing integers
        - DECIMAL(precision, decimals): numbers with total number of digits equal to precision and decimal places equal to decimals
        - NUMERIC(precision, decimal points): numbers with total number of digits equal to precision and decimal places equal to decimals
        - CHAR(limit): a string with a limited number of chars. Shorter strings are padded with spaces
        - VARCHAR(limit): a string with a limited number of chars. Shorter strings are not padded
        - TEXT: string of any length
        - TIME: a time of day
        - DATE: a calendar date
        - TIMESTAMP: combination of calendar date + time in one field
        - *REAL*
      * used with SET
    - NOT NULL
      - ensures that a column cannot contain null values
      - used with SET

- Data Manipulation Language
  * SQL that is used to handle the data that's stored in the database
  * Includes the following commands/keywords
    - SELECT: retrieves values from specified columns in a table
      * FROM: the table that records are to be retrieved from
      * GROUP BY: determines the way the records that are included in the selection are grouped together
        - used with aggregate functions
        - *cannot contain an aggregate function itself, use HAVING for that*
      * WHERE: specifies the conditions that the records must meet to be included in the selection
        - used before GROUP BY
      * HAVING: specifies the conditions that the groups of records must meet to be included in the selection
        - used after GROUP BY
      * ORDER BY: determines columns that are used to sort the records that are included in the selection 
        - ASC: ascending order
        - DESC: descending order
    - JOIN (INNER, LEFT, RIGHT, FULL, CROSS)
      * joins multiple tables that share a common relationship together based on different parameters (more later)
    - INSERT: specifies the column names into which sets of values need to be inserted
      * INTO: specifies the table that columns belong to
      * VALUES: specifies the sets of values to be inserted, each entry wrapped in parentheses and separated by a comma
    - UPDATE: specifies a column of a table in the database in which values need to be updated
      * SET: specifies the value that the updated records should have in the specified column
      * WHERE: specifies the conditions that are used to select the values from the column that need to be updated 
    - DELETE: removes records from a table in the database  
      * FROM: the table that the records are meant to be removed from
      * WHERE: the conditions that the records that are to be removed need to meet

- Data Control Language
  * used to define the access rights of users who use a database
  * common commands include GRANT and REVOKE
  * not covered extensively in LS180

- Functions
  - Aggregate functions
    * count()
    * min()
    * max()
    * avg()
  - Length()

### Write SQL statements using SELECT, INSERT, UPDATE, DELETE, CREATE/ALTER/DROP TABLE, ADD/ALTER/DROP COLUMN
### Understand how to create and remove constraints, including CHECK constraints
### Understand how to use GROUP BY, ORDER BY, WHERE, and HAVING

- Placeholders are enclosed in {}

- CREATE TABLE {table_name}({first column usually: id} {first_column data type usually: SERIAL} {first column usually: PRIMARY KEY}, {next_column} {next_column data type} {optional next column: UNIQUE} {optional next column: NOT NULL} {DEFAULT {default_value}});
- ALTER TABLE {table name} {possible operations see below}
  - ADD/DROP/ALTER COLUMN {column_name}
  - ADD/DROP CONSTRAINT {constraint_name}
- DROP TABLE {table_name};

- ALTER TABLE {table_name} RENAME TO {new_table_name};
- ALTER TABLE {table_name} RENAME COLUMN {old_column_name} TO {new_column_name};
- ALTER TABLE {table_name} ALTER COLUMN {column_name} TYPE {new_type};
- ALTER TABLE {table_name} ADD COLUMN {column_name} {column_data_type} {optional: UNIQUE} {optional: NOT NULL} {optional: DEFAULT {default_value}};
- ALTER TABLE {table_name} DROP COLUMN {column_name};
- ALTER TABLE {table_name} ADD CONSTRAINT {optional: constraint_name} UNIQUE(column_name);
- ALTER TABLE {table_name} ADD CONSTRAINT {optional: constraint_name} CHECK(condition);
- ALTER TABLE {table_name} ADD CONSTRAINT {foreign_key_name} FOREIGN KEY (foreign_key_column) REFERENCES {other_table(column_name)} {optional: ON DELETE CASCADE};
- ALTER TABLE {talbe_name} DROP CONSTRAINT {constraint_name};
- ALTER TABLE {table_name} ALTER COLUMN {column_name} SET NOT NULL;
- ALTER TABLE {table_name} ALTER COLUMN {column_name} DROP NOT NULL;
- ALTER TABLE {table_name} ALTER COLUMN {column_name} SET DEFAULT {new_default_value};
- ALTER TABLE {table_name} ALTER COLUMN {column_name} DROP DEFAULT;


- SELECT {wildcard: *} {column1}, {column2}, {...additional columns} FROM {table_name} {optional: WHERE condition} {optional: GROUP BY column_name} {optional: HAVING condition} {optional: ORDER BY column_name ASC/DESC};
  * wildcard *: selects all columns from table
  * WHERE condition: used to select records from column before GROUP BY
  * GROUP BY: used to group records together in the output
    - necessary when using aggregate functions alongside non-aggregated columns 
  * HAVING: used to select groups after GROUP BY (*can* be used alongside WHERE)
  * ORDER BY: sorts output according to values in a particular column
    - ASC: ascending order
    - DESC: descending order
  * Aggregate functions:
    - Used to compute aggregate values for an entire column or for groups
    - Includes: count(), min(), max(), avg(), string_agg({column_}, {aggregator})
    - GROUP BY needs to be used to group other columns that are not being aggregated
    - *Multiple* aggregates functions per query are possible

- INSERT INTO {table_name}({column1_name}, {optional: column2_name}, {optional: ...additional columns}) VALUES ({record1_value_column1}, {record1_value_column2}, ...{record1_additional_columns}), ({record2_value_column1}, {record2_value_column2}, ...{record2_additional_columns}), {...additional records};

- DELETE FROM {table_name} WHERE {condition1, e.g. column_name = target value} {optional: AND} {optional: ...additional conditions};
  * without a WHERE clause all rows will be deleted!

- UPDATE {table_name} SET {column_name} = {new_value} WHERE condition;

### Identify the different types of JOINs and explain their differences

- INNER JOIN: only returns records for which the join condition matches in both tables
- LEFT JOIN: returns all records that exist in the left table, even if there's no corresponding record in the right table. 
  * Unmatched right-side columns will be filled with NULL
- RIGHT JOIN: returns all records that exist in the right table, even if there's no corresponding record in the left table. 
  * Unmatched left-side columns will be filled with NULL
- FULL JOIN: returns all records from both tables, even if they don't have a corresponding record in the other table
  * Unmatched columns from either side are filled with NULL.
- CROSS JOIN: returns all possible combinations of records between the tables, even particular combinations of records that are not actually related
  * cartesian product: refers to all possible combinations of records between the tables

### Be familiar with using subqueries and join tables

- Subqueries always wrapped in brackets

- Selecting a column based on a condition in a related table
  * SELECT column_name FROM table1 WHERE table1.matching_column IN (SELECT table2.matching_column FROM table2 WHERE condition);
  * SELECT column_name FROM table1 WHERE table1.matching_column NOT IN (SELECT table2.matching_column FROM table2 WHERE condition);

- Scalar subqueries
  * return a single record that can be used by the outer query
  * used to mimic an INNER JOIN

- Sub queries with EXISTS
  * 
  * SELECT column_name FROM table1_name WHERE EXISTS (SELECT 1 FROM table2_name WHERE table1.join_column = table2.join_column);

- Using a subquery to generate a lookup table
  * SELECT alias.column1_name FROM (SELECT column2_name FROM table1 WHERE condition) AS alias

- Join tables
  * stores foreign keys to two related tables
  * usually used to implement a many-to-many relationship

### Row constructor

- creats a row entity that can be compared to actual records
- SELECT count(id) FROM table1 WHERE row(table1.column2, table1.column5, table1.column 8) = row ('Book', 20, 'In Stock');

## PostgreSQL

### Describe what a sequence is and what they are used for
### Create an auto-incrementing column

- A Postgres *database object* that generates a sequence of numbers; `nextval()` retrieves the next value and advances the sequence
- Created using:
  * CREATE SEQUENCE sequence_name START WITH {number_x} INCREMENT BY {number_y};
- Subsequent values are generated through nextval('sequence_name') function calls
- Often used to create series of unique numbers to be used as a primary key
  * CREATE TABLE table_name(id INTEGER DEFAULT nextval('sequence_name') PRIMARY KEY);
  * same as CREATE TABLE table_name(id SERIAL PRIMARY KEY);
  * SERIAL creates a sequence that starts from 1 and increments by 1
- ALTER SEQUENCE sequence_name RESTART WITH number_p INCREMENT BY number_q;

### Define a default value for a column

- When defined, default values are used when records are inserted into a table without providing values for all columns in that table (defaults are used for missing columns)
- They are defined as follows
  * CREATE table_name(column_name data_type DEFAULT default_value);
  * ALTER TABLE table_name ALTER COLUMN column_name SET DEFAULT default_value;
  * ALTER TABLE table_name ALTER COLUMN column_name DROP DEFAULT;

### Be able to describe what primary, foreign, natural, and surrogate keys are

- primary key: required when defined, unique, non-null values that are used to unambiguously identify individual records in a table
- natural key: a column whose existing values already meet the criteria to be a primary key
- surrogate key: a column that is added to the database to serve as a primary key if no columns naturally meet the criteria
- foreign key: a column that maintains a relationship with another table by referencing identifiers that identify matching records in the other table
  * not required by default, can be null by default, not necessarily unique values by default
  * usually references a primary key column in another table


### Create and remove CHECK constraints from a column
### Create and remove foreign key constraints from a column
- ALTER TABLE {table_name} ADD CONSTRAINT {optional: constraint_name} UNIQUE(column_name);
- ALTER TABLE {table_name} ADD CONSTRAINT {optional: constraint_name} CHECK(condition);
- ALTER TABLE {table_name} ADD CONSTRAINT {foreign_key_name} FOREIGN KEY (foreign_key_column) REFERENCES ({other_table.column_name}) {optional: ON DELETE CASCADE};
- ALTER TABLE {table_name} DROP CONSTRAINT {constraint_name};


## Database Diagrams

### Talk about the different levels of schema.

- Conceptual level: high level descriptions of the entities and the relationships between them (modality, cardinality)
- Logical level: model tables, columns/attributes, keys, and relationships in a database-structured way, but still not necessarily tied to physical storage details.
- Physical level: description of the actual implementation of the database schema (tables, columns, keys, relationships)
- Entity Relationship Diagram (ERD): graphically displays the relationships and their cardinality and modality

### Define cardinality and modality.

- Cardinality:
  * Whether each side of the relationship can have one or many records
  * Cardinality of 1 is marked by a straight line in an ERD
  * Cardinality of many is marked by a crow's foot in an ERD
  * As a result, relationships can be one-to-one, one-to-many or many-to-many
- Modality
  * Whether related records are required or not on either side of the relationship
  * Modality of 1 means that for every record in a table, there has to be at least one related record in the other table
  * Modality of 0 means that every record in a table can have zero matching records in another table
  * In an ERD
    - Modality of 1 is represented by a line that crosses the relationship perpendicularly (before the crow's foot)
    - Modality of 0 is represented by a circle

- Essentially, cardinality and modality determine max and min number of records on either side of the relationship
  - Cardinality of 1 -> at most one record on that side
  - Cardinality of many -> many records possible on that side
  - Modality of 0: there can be zero matching records on that side
  - Modality of 1: there has to be at least 1 matching record for each record on that side

### Be able to draw database diagrams using crow's foot notation.


### Be able to interpret existing ERDs and crow's foot notation


## Extra
- **Any comparison to NULL using standard operators like = will result in NULL, not false. Even comparing NULL = NULL results in NULL. Because the result of the comparison is NULL (which represents an unknown value), it's treated as "not true" in contexts like a WHERE clause, which is why you need the special IS NULL operator to specifically test for NULL.**



- Query performance
- Indexing
- EXPLAIN/EXPLAIN ANALYZE





- String quotation rules
- String concatenation operator
- String lower case 
- Int truncation
- Marking arbitrary statements (such as calculation)
- sql meta commands

- extract("year" from current_date)
- create sequence sequence_name
- nextval('sequence_name');
- create sequence counter increment by 2 minvalue 2;

- levels of schema

- cardinality vs modality

- cast

- foreign keys can be null

- update anomaly
- insertion anomaly
- deletion anomaly

- columns that are named with words that mirror sql identifiers - use "column_name" in query

- bracket vs dot notation table.column vs. table(column)

- many-to-many
  * Why can't we simply add a foreign key column to either the books or categories table to represent the relationship between them?

  
explain
explain analyze
