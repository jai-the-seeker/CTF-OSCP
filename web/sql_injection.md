* [`sqlmap`](#sqlmap)
* [INFORMATION_SCHEMA](#information_schema)

# `sqlmap`

# INFORMATION_SCHEMA
Refs:
* <https://www.cmi.ac.in/~madhavan/courses/databases10/mysql-5.0-reference-manual/information-schema.html#schemata-table>

INFORMATION_SCHEMA provides access to database metadata.

Metadata is data about the data, such as the name of a database or table, the data type of a column, or access privileges. INFORMATION_SCHEMA is the information database, the place that stores information about all the other databases that the MySQL server maintains. Inside INFORMATION_SCHEMA there are several read-only tables. They are actually views, not base tables, so there are no files associated with them.
