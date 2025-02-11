# pinnacledb Documentation

## Overview

##### what is PinnacleDB?
PinnacleDB is a lightweight, file-based database management system that allows users to create, manipulate, and retrieve data efficiently. It uses serialization for storage due to the PinnacleSerializer and provides transaction-based operations.

##### How it works?
When you first instantiate a PinnacleDB object, the constructor checks whether the corresponding database file (with a “.pdb” extension) exists. If it does not exist, the code creates the file by serializing an initial dictionary structure. This process ensures that the database has a well-defined starting state. For example, the current implementation creates a basic dictionary like this:
```python
{'tables': {}}
```
This dictionary serves as the foundation for storing all database tables and their associated data. The PinnacleSerializer.serialize function is then used to write this dictionary to the file, and PinnacleSerializer.deserialize is used to load it back into memory when the database is accessed. This read/write mechanism maintains data persistence across sessions.

Underlying Data Structure
PinnacleDB uses a specific dictionary structure to manage its data. This structure is designed to store tables, columns, and rows in a straightforward way. Here’s an example of the dictionary structure that PinnacleDB might use:
```python
PinnacleDB_structure = {
    'tables': {
        'users': {
            'columns': ['name', 'age'],
            'rows': {
                1: ('omar', 15),
                2: ('aya', 16),
            }
        }
    }
}

```
###### How This Structure Works:
- tables Key:
The root dictionary has a key named "tables", which holds all the tables in the database. Each table is itself a dictionary.

- Table Dictionary:
In this example, there is a table called "users". Each table dictionary contains:

  - columns: A list that defines the names of the columns for that table (e.g., ['name', 'age']).
  - rows: A dictionary where each key represents a unique identifier for a row (for instance, numeric IDs like 1 and 2), and each value is a tuple containing the row’s data corresponding to the columns.

- Extensibility:
This structure is designed to be extended easily. As you create additional tables or insert more rows, the "tables" dictionary will simply have more keys, and each table can grow with its own set of columns and rows.
###### Putting It All Together
- Initialization:
When a new PinnacleDB instance is created, the code checks for the existence of a database file. If the file is missing, it creates one by serializing a starting dictionary (initially empty tables) using PicleSerializer.serialize.

- Serialization and Deserialization:
The serialized file contains the entire state of the database (in the form of a dictionary following the structure shown above). Every time the database is modified (e.g., tables are created, rows are added or updated), the in-memory dictionary is re-serialized to the file, ensuring that all changes are saved.

- Data Operations:
All methods within PinnacleDB (such as creating a table, inserting a row, or fetching data) work directly on this in-memory dictionary. The operations manipulate the "tables", "columns", and "rows" keys as defined in the structure. When needed, the updated dictionary is written back to the file to persist the changes.

###### This design using a well-structured dictionary for storing tables and data makes PinnacleDB both simple and effective for lightweight database operations. It allows you to easily manage, backup, and restore your data while keeping the underlying data model transparent and extensible.
## Author Information

- **Version**: Beta
- **Author**: Chtaïba Omar
- **Licence** : MIT Licence
- **Email**: [omarchtaiba4@gmail.com](mailto\:omarchtaiba4@gmail.com)
- **GitHub**: [PinnacleDB Repository](https://github.com/Omar1CHTAIBA)
- **X**: [@OmarChtaib90132](https://x.com/OmarChtaib90132)
- **Date**: 2/9/2025

## Features

- 28 methods are available to make you more comfortable dealing with your database system
- CRUD operations
- Transaction-based operations
- Backup and restore functionality
- CSV export
- printing tables in terminal

## Class: `PinnacleDB`

### Initialization

```python
PinnacleDB(db_name)
```

- **db\_name** *(str)*: The name of the database file (.pdb).

### Methods

#### 1. `__str__()`

Returns metadata information about the database (author, version, etc.).

#### 2. `_sync()`

Synchronizes in-memory data with the file storage.

#### 3. `call()`

Starts a transaction. Raises an `AlreadyCalledError` if a transaction is already active.

#### 4. `show_commands()`

Displays the list of queued commands within a transaction.

#### 5. `delete_command(index)`

Deletes a command from the transaction log.

- **index** *(int)*: The index of the command to remove.

#### 6. `fold()`

Processes all queued commands in a transaction and commits them to the database.

#### 7. `create_table(table_name, columns)`

Creates a table with specified columns.

- **table\_name** *(str)*: The name of the table.
- **columns** *(list)*: Column names.

#### 8. `set(table_name, key, *values)`

Inserts or updates a row in a table.

- **table\_name** *(str)*: Table name.
- **key** *(str)*: Unique identifier for the row.
- **values** *(tuple)*: Row values.

#### 9. `delete(table_name, key)`

Deletes a row by key.

- **table\_name** *(str)*: Table name.
- **key** *(str)*: Row key.

#### 10. `fetch(table_name, key)`

Fetches a specific row by key.

#### 11. `len_tables()`

Returns the number of tables in the database.

#### 12. `fetchtables()`

Returns a list of table names.

#### 13. `count(table_name)`

Returns the number of rows in a table.

#### 14. `fetchall(table_name, limit=None, offset=None)`

Fetches all rows, with optional limit and offset.

#### 15. `fetchrows(table_name)`

Returns all rows in a table.

#### 16. `fetchcolumns(table_name)`

Returns the column names of a table.

#### 17. `fetchkeys(table_name)`

Returns all row keys in a table.

#### 18. `fetchvalues(table_name)`

Returns all row values in a table.

#### 19. `get_column_names(table_name)`

Returns column names of a table.

#### 20. `fetchrows_by_value(table_name, value)`

Finds rows that contain a specific value.

#### 21. `fetchrows_by_key(key)`

Finds rows across all tables with a specific key.

#### 22. `clear()`

Clears all data from the database.

#### 23. `delete_table(table_name)`

Deletes a table.

#### 24. `print_table(columns, rows)`

Prints a table to the console.

#### 25. `export_to_csv(table_name, file_path)`

Exports a table to a CSV file.

#### 26. `backup(backup_name)`

Creates a backup of the database.

#### 27. `restore(backup_name)`

Restores the database from a backup.

#### 28. `close()`

Synchronizes and closes the database.

## Error Handling

- `PinnacleError`
- `TableNotFoundError`
- `RowNotFoundError`
- `TableAlreadyExistsError`
- `ColumnMismatchError`
- `DeletingCommandError`
- `AlreadyCalledError`
- `NoCommandWasFoundError`
- `ParsingError`

## Usage Example

```python
from pinnacledb import PinnacleDB

db = PinnacleDB("my_database")
db.create_table("Users", ["Name", "Age"])
db.set("Users", "user1", "Alice", 25)
print(db.fetch("Users", "user1"))
db.export_to_csv("Users", "users.csv")
db.print_table(db.fetchcolumns("Users"), db.fetchrows("Users"))
db.close()
```

### Additional Usage Examples

#### Fetching all rows from a table

```python
users = db.fetchall("Users")
print(users)
```

#### Fetching rows by value

```python
matching_rows = db.fetchrows_by_value("Users", 25)
print(matching_rows)
```

## Transaction Example

```python
from pinnacledb import PinnacleDB

db = PinnacleDB("test_db")
db.create_table("Employees", ["Name", "Position"])

db.call()  # Start transaction
db.set("Employees", "e1", "John Doe", "Manager")
db.set("Employees", "e2", "Jane Smith", "Developer")
db.show_commands()  # Displays queued commands
db.fold()  # Commit transaction
db.close()
```

## Advanced Transaction Example

```python
from pinnacledb import PinnacleDB

db = PinnacleDB("company_db")
db.create_table("Departments", ["DeptName", "Location"])
db.create_table("Employees", ["Name", "Age", "Department"])

db.call()  # Start transaction
db.querying.append("CREATE TABLE Departments ['DeptName', 'Location']")
db.querying.append("CREATE TABLE Employees ['Name', 'Age', 'Department']")
db.querying.append("SET Employees e1 ['Alice', 30, 'HR']")
db.querying.append("SET Employees e2 ['Bob', 28, 'Engineering']")
db.querying.append("SET Departments d1 ['HR', 'Building A']")
db.querying.append("SET Departments d2 ['Engineering', 'Building B']")
db.querying.append("DELETE Employees e1")
db.querying.append("DELETE__ Departments")
db.querying.append("BACKUP company_backup")
db.querying.append("RESTORE company_backup")
db.querying.append("CLEAR DATABASE")
db.show_commands()  # Displays all queries before committing
db.fold()  # Commit transaction
db.close()
```
## License
```sql
MIT License

Copyright (c) 2025 Chtaïba Omar

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

```

## Conclusion

PinnacleDB is a simple, yet powerful database system for lightweight applications requiring persistent storage. Its transaction support and serialization ensure reliability and data integrity.

