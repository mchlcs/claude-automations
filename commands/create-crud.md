Generate a complete CRUD in Java for the entity: $ARGUMENTS

> **Note:** User input and output can be in PT-BR. Prompts are English for portability.

Use the DAO (Data Access Object) pattern. Include:

## Structure to Generate

### 1. Model (`$ARGUMENTS.java`)
- Private attributes with appropriate types
- Full constructor and empty constructor
- Getters and Setters
- `toString()` for debugging

### 2. DAO Interface (`I$ARGUMENTSDao.java`)
- `create($ARGUMENTS obj)`
- `findById(int id)`
- `findAll()`
- `update($ARGUMENTS obj)`
- `delete(int id)`

### 3. DAO Implementation (`$ARGUMENTSDaoImpl.java`)
- Implements the DAO interface
- Uses JDBC with `PreparedStatement` (never concatenate SQL)
- Handles `SQLException` properly
- Closes connections in `finally` block or uses try-with-resources

### 4. SQL (`create_table_$ARGUMENTS.sql`)
- CREATE TABLE with Oracle SQL types
- PRIMARY KEY with SEQUENCE + TRIGGER (Oracle pattern)
- Relevant constraints (NOT NULL, VARCHAR2)

### Connection Pattern
Use `ConnectionFactory.getConnection()`.

Generate functional, compilable code.
