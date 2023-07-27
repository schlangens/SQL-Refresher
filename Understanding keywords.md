## INSERT
[INSERT](local:INSERT) inserts new rows into an existing table. The ``INSERT ... VALUES`` and ``INSERT ... SET`` forms of the statement insert rows based on explicitly specified values. The ``INSERT ... SELECT`` form inserts rows selected from another table or tables. [INSERT](local:INSERT) with an ON DUPLICATE KEY UPDATE clause enables existing rows to be updated if a row to be inserted would cause a duplicate value in a ``UNIQUE`` index or ``PRIMARY`` KEY.

For additional information about ``INSERT ... SELECT and INSERT ... ON DUPLICATE KEY UPDATE``, see [insert-select](http://dev.mysql.com/doc/refman/8.0/en/insert-select.html), and [insert-on-duplicate](http://dev.mysql.com/doc/refman/8.0/en/insert-on-duplicate.html).

In MySQL current-series, the ``DELAYED`` keyword is accepted but ignored by the server. For the reasons for this, see [insert-delayed](http://dev.mysql.com/doc/refman/8.0/en/insert-delayed.html),

Inserting into a table requires the ``INSERT`` privilege for the table. If the ON DUPLICATE KEY ``UPDATE`` clause is used and a duplicate key causes an [UPDATE](local:UPDATE) to be performed instead, the statement requires the ``UPDATE`` privilege for the columns to be updated. For columns that are read but not modified you need only the ``SELET`` privilege (such as for a column referenced only on the right hand side of an ``col_name=expr`` assignment in an ``ON DUPLICATE KEY UPDATE``clause).


## CREATE TABLE Syntax:

[CREATE TABLE](local:CREATE TABLE) creates a table with the given name. You must have the ``CREATE`` privilege for the table.

By default, tables are created in the default database, using the InnoDB storage engine. An error occurs if the table exists, if there is no default database, or if the database does not exist.

MySQL has no limit on the number of tables. The underlying file system may have a limit on the number of files that represent tables. Individual storage engines may impose engine-specific constraints. InnoDB permits up to 4 billion tables.

For information about the physical representation of a table, see [create-table-files](http://dev.mysql.com/doc/refman/8.0/en/create-table-files.html).

**See also:** : [Online help create-table](http://dev.mysql.com/doc/refman/8.0/en/create-table.html)


## UPDATE Syntax:

For the single-table syntax, the [UPDATE](local:UPDATE) statement updates columns of existing rows in the named table with new values. The SET clause indicates which columns to modify and the values they should be given. Each value can be given as an expression, or the keyword ``DEFAULT`` to set a column explicitly to its default value. The ``WHERE`` clause, if given, specifies the conditions that identify which rows to update. With no ``WHERE`` clause, all rows are updated. If the ``ORDER BY`` clause is specified, the rows are updated in the order that is specified. The ``LIMIT`` clause places a limit on the number of rows that can be updated.

For the multiple-table syntax, [UPDATE](local:UPDATE) updates rows in each table named in ``table_references`` that satisfy the conditions. Each matching row is updated once, even if it matches the conditions multiple times. For multiple-table syntax, ``ORDER BY`` and ``LIMIT`` cannot be used.

For partitioned tables, both the single-single and multiple-table forms of this statement support the use of a ``PARTITION`` option as part of a table reference. This option takes a list of one or more partitions or subpartitions (or both). Only the partitions (or subpartitions) listed are checked for matches, and a row that is not in any of these partitions or subpartitions is not updated, whether it satisfies the where_condition or not.

For more information and examples, see [partitioning-selection](http://dev.mysql.com/doc/refman/8.0/en/partitioning-selection.html).

where_condition is an expression that evaluates to true for each row to be updated. For expression syntax, see [expressions](http://dev.mysql.com/doc/refman/8.0/en/expressions.html).

table_references and where_condition are specified as described in [select](http://dev.mysql.com/doc/refman/8.0/en/select.html).

You need the ``UPDATE`` privilege only for columns referenced in an [UPDATE](local:UPDATE) that are actually updated. You need only the SELECT privilege for any columns that are read but not modified.

The [UPDATE](local:UPDATE) statement supports the following modifiers:

- With the ``LOW_PRIORITY`` modifier, execution of the [UPDATE](local:UPDATE) is delayed until no other clients are reading from the table. This affects only storage engines that


##  DELETE Syntax:

You need the ``DELETE`` privilege on a table to delete rows from it. You need only the ``SELECT`` privilege for any columns that are only read, such as those named in the ``WHERE`` clause.

When you do not need to know the number of deleted rows, the [TRUNCATE TABLE](local:TRUNCATE TABLE) statement is a faster way to empty a table than a [DELETE](local:DELETE) statement with no WHERE clause. Unlike [DELETE](local:DELETE), [TRUNCATE TABLE](local:TRUNCATE TABLE) cannot be used within a transaction or if you have a lock on the table. See [truncate-table](http://dev.mysql.com/doc/refman/8.0/en/truncate-table.html) and [lock-tables](http://dev.mysql.com/doc/refman/8.0/en/lock-tables.html).

The speed of delete operations may also be affected by factors discussed in [delete-optimization](http://dev.mysql.com/doc/refman/8.0/en/delete-optimization.html).

To ensure that a given [DELETE](local:DELETE) statement does not take too much time, the MySQL-specific LIMIT row_count clause for [DELETE](local:DELETE) specifies the maximum number of rows to be deleted. If the number of rows to delete is larger than the limit, repeat the ``DELETE`` statement until the number of affected rows is less than the LIMIT value.

You cannot delete from a table and select from the same table in a subquery.

``DELETE`` supports explicit partition selection using the ``PARTITION`` option, which takes a list of the comma-separated names of one or more partitions or subpartitions (or both) from which to select rows to be dropped. Partitions not included in the list are ignored. Given a partitioned table t with a partition named p0, executing the statement ``DELETE FROM t PARTITION (p0)`` has the same effect on the table as executing ``ALTER TABLE t TRUNCATE PARTITION (p0);`` in both cases, all rows in partition p0 are dropped.

PARTITION can be used along with a WHERE condition, in which case the condition is tested only on rows in the listed partitions. For example, DELETE FROM t PARTITION (p0) WHERE c < 5 deletes rows only from partition p0 for which the condition c < 5 is true; rows in any other partitions are not checked and thus not affected by the DELETE.

The PARTITION option can also be used in multiple-table DELETE statements. You can use up to one such option per table named in the FROM option.

For more information and examples, see [partitioning-selection](http://dev.mysql.com/doc/refman/8.0/en/partitioning-selection.html).

**See also:** : [Online help delete](http://dev.mysql.com/doc/refman/8.0/en/delete.html)

## CREATE VIEW Syntax:

The [CREATE VIEW](local:CREATE VIEW) statement creates a new view, or replaces an existing view if the ``OR REPLACE`` clause is given. If the view does not exist, ``CREATE OR REPLACE VIEW`` is the same as ``CREATE VIEW`` If the view does exist, ``CREATE OR REPLACE VIEW`` replaces it.

For information about restrictions on view use, see [view-restrictions](http://dev.mysql.com/doc/refman/8.0/en/view-restrictions.html).

The`` select_statement`` is a [SELECT](local:SELECT) statement that provides the definition of the view. (Selecting from the view selects, in effect, using the [SELECT](local:SELECT) statement.) The select_statement can select from base tables or other views.

The view definition is frozen at creation time and is not affected by subsequent changes to the definitions of the underlying tables. For example, if a view is defined as ``SELECT *`` on a table, new columns added to the table later do not become part of the view, and columns dropped from the table will result in an error when selecting from the view.

The ALGORITHM clause affects how MySQL processes the view. The DEFINER and SQL SECURITY clauses specify the security context to be used when checking access privileges at view invocation time. The WITH CHECK OPTION clause can be given to constrain inserts or updates to rows in tables referenced by the view. These clauses are described later in this section.

The [CREATE VIEW](local:CREATE VIEW) statement requires the CREATE VIEW privilege for the view, and some privilege for each column selected by the [SELECT](local:SELECT) statement. For columns used elsewhere in the [SELECT](local:SELECT) statement, you must have the ``SELECT`` privilege. If the OR REPLACE clause is present, you must also have the DROP privilege for the view. If the ``DEFINER`` clause is present, the privileges required depend on the user value, as discussed in [stored-objects-security](http://dev.mysql.com/doc/refman/8.0/en/stored-objects-security.html).

When a view is referenced, privilege checking occurs as described later in this section.

A view belongs to a database. By default, a new view is created in the default database. To create the view explicitly in a given database, use db_name.view_name syntax to qualify the view name with the database name:

>
CREATE VIEW test.v AS SELECT * FROM t;

  

Unqualified table or view names in the [SELECT](local:SELECT) statement are also interpreted with respect to the default database. A view can refer to tables or views in other databases by qualifying the table or view name with the appropriate database name.

Within a database, base tables and views share the same namespace, so a base table and a view cannot have the same name.

Columns retrieved by the [SELECT](local:SELECT) statement can be simple references to table columns, or expressions that use functions, constant values, operators, and so forth.

A view must have unique column names with no duplicates, just like a base table. By default, the names of the columns retrieved by the [SELECT](local:SELECT) statement are used for the view column names. To define explicit names for the view columns, specify the optional column_list clause as a list of comma-separated identifiers. The number of names in column_list must be the same as the number of columns retrieved by the [SELECT](local:SELECT) statement.

A view can be created from many kinds of [SELECT](local:SELECT) statements. It can refer to base tables or other views. It can use joins, [UNION](local:UNION), and subqueries. The [SELECT](local:SELECT) need not even refer to any tables:

>
CREATE VIEW v_today (today) AS SELECT CURRENT_DATE;

  

The following example defines a view that selects two columns from another table as well as an expression calculated from those columns:

>
mysql> CREATE TABLE t (qty INT, price INT);
mysql> INSERT INTO t VALUES(3, 50);
mysql> CREATE VIEW v AS SELECT qty, price, qty*price AS value FROM t;
mysql> SELECT * FROM v;
+------+-------+-------+
| qty  | price | value |
+------+-------+-------+
|    3 |    50 |   150 |
+------+-------+-------+

  

A view definition is subject to the following restrictions:

- The [SELECT](local:SELECT) statement cannot refer to system variables or user-defined variables.
    
- Within a stored program, the [SELECT](local:SELECT) statement cannot refer to program parameters or local variables.
    
- The [SELECT](local:SELECT) statement cannot refer to prepared statement parameters.
    
- Any table or view referred to in the definition must exist. If, after the view has been created, a table or view that the definition refers to is dropped, use of the view results in an error. To check a view definition for problems of this kind, use the [CHECK TABLE](local:CHECK TABLE) statement.
    
- The definition cannot refer to a TEMPORARY table, and you cannot create a TEMPORARY view.
    
- You cannot associate a trigger with a view.
    
- Aliases for column names in the [SELECT](local:SELECT) statement are checked against the maximum column length of 64 characters (not the maximum alias length of 256 characters).
    

ORDER BY is permitted in a view definition, but it is ignored if you select from a view using a statement that has its own ORDER BY.

For other options or clauses in the definition, they are added to the options or clauses of the statement that references the view, but the effect is undefined. For example, if a view definition includes a LIMIT clause, and you select from the view using a statement that has its own LIMIT clause, it is undefined which limit applies. This same principle applies to options such as ALL, DISTINCT, or SQL_SMALL_RESULT that follow the [SELECT](local:SELECT) keyword, and to clauses such as INTO, FOR UPDATE, FOR SHARE, LOCK IN SHARE MODE, and PROCEDURE.

The results obtained from a view may be affected if you change the query processing environment by changing system variables:

>

mysql> CREATE VIEW v (mycol) AS SELECT 'abc';
Query OK, 0 rows affected (0.01 sec)

mysql> SET sql_mode = '';
Query OK, 0 rows affected (0.00 sec)

mysql> SELECT "mycol" FROM v;
+-------+
| mycol |
+-------+
| mycol |
+-------+
1 row in set (0.01 sec)

mysql> SET sql_mode = 'ANSI_QUOTES';
Query OK, 0 rows affected (0.00 sec)

mysql> SELECT "mycol" FROM v;
+-------+
| mycol |
+-------+
| abc   |
+-------+
1 row in set (0.00 sec)

  

The DEFINER and SQL SECURITY clauses determine which MySQL account to use when checking access privileges for the view when a statement is executed that references the view. The valid SQL SECURITY characteristic values are DEFINER (the default) and INVOKER. These indicate that the required privileges must be held by the user who defined or invoked the view, respectively.

If the DEFINER clause is present, the user value should be a MySQL account specified as 'user_name'@'host_name', CURRENT_USER, or CURRENT_USER(). The permitted user values depend on the privileges you hold, as discussed in [stored-objects-security](http://dev.mysql.com/doc/refman/8.0/en/stored-objects-security.html). Also see that section for additional information about view security.

If the DEFINER clause is omitted, the default definer is the user who executes the [CREATE VIEW](local:CREATE
      VIEW) statement. This is the same as specifying DEFINER = CURRENT_USER explicitly.

Within a view definition, the CURRENT_USER function returns the view's DEFINER value by default. For views defined with the SQL SECURITY INVOKER characteristic, CURRENT_USER returns the account for the view's invoker. For information about user auditing within views, see [account-activity-auditing](http://dev.mysql.com/doc/refman/8.0/en/account-activity-auditing.html).

Within a stored routine that is defined with the SQL SECURITY DEFINER characteristic, CURRENT_USER returns the routine's DEFINER value. This also affects a view defined within such a routine, if the view definition contains a DEFINER value of CURRENT_USER.

MySQL checks view privileges like this:

- At view definition time, the view creator must have the privileges needed to use the top-level objects accessed by the view. For example, if the view definition refers to table columns, the creator must have some privilege for each column in the select list of the definition, and the SELECT privilege for each column used elsewhere in the definition. If the definition refers to a stored function, only the privileges needed to invoke the function can be checked. The privileges required at function invocation time can be checked only as it executes: For different invocations, different execution paths within the function might be taken.
    
- The user who references a view must have appropriate privileges to access it (SELECT to select from it, INSERT to insert into it, and so forth.)
    
- When a view has been referenced, privileges for objects accessed by the view are checked against the privileges held by the view DEFINER account or invoker, depending on whether the SQL SECURITY characteristic is DEFINER or INVOKER, respectively.
    
- If reference to a view causes execution of a stored function, privilege checking for statements executed within the function depend on whether the function SQL SECURITY characteristic is DEFINER or INVOKER. If the security characteristic is DEFINER, the function runs with the privileges of the DEFINER account. If the characteristic is INVOKER, the function runs with the privileges determined by the view's SQL SECURITY characteristic.
    

Example: A view might depend on a stored function, and that function might invoke other stored routines. For example, the following view invokes a stored function f():

>
CREATE VIEW v AS SELECT * FROM t WHERE t.id = f(t.name);

  

Suppose that f() contains a statement such as this:

>
IF name IS NULL then
  CALL p1();
ELSE
  CALL p2();
END IF;

  

The privileges required for executing statements within f() need to be checked when f() executes. This might mean that privileges are needed for p1() or p2(), depending on the execution path within f(). Those privileges must be checked at runtime, and the user who must possess the privileges is determined by the SQL SECURITY values of the view v and the function f().

The DEFINER and SQL SECURITY clauses for views are extensions to standard SQL. In standard SQL, views are handled using the rules for SQL SECURITY DEFINER. The standard says that the definer of the view, which is the same as the owner of the view's schema, gets applicable privileges on the view (for example, SELECT) and may grant them. MySQL has no concept of a schema owner, so MySQL adds a clause to identify the definer. The DEFINER clause is an extension where the intent is to have what the standard has; that is, a permanent record of who defined the view. This is why the default DEFINER value is the account of the view creator.

The optional ALGORITHM clause is a MySQL extension to standard SQL. It affects how MySQL processes the view. ALGORITHM takes three values: MERGE, TEMPTABLE, or UNDEFINED. For more information, see [view-algorithms](http://dev.mysql.com/doc/refman/8.0/en/view-algorithms.html), as well as [derived-table-optimization](http://dev.mysql.com/doc/refman/8.0/en/derived-table-optimization.html).

Some views are updatable. That is, you can use them in statements such as [UPDATE](local:UPDATE), [DELETE](local:DELETE), or [INSERT](local:INSERT) to update the contents of the underlying table. For a view to be updatable, there must be a one-to-one relationship between the rows in the view and the rows in the underlying table. There are also certain other constructs that make a view nonupdatable.

A generated column in a view is considered updatable because it is possible to assign to it. However, if such a column is updated explicitly, the only permitted value is DEFAULT. For information about generated columns, see [create-table-generated-columns](http://dev.mysql.com/doc/refman/8.0/en/create-table-generated-columns.html).

The WITH CHECK OPTION clause can be given for an updatable view to prevent inserts or updates to rows except those for which the WHERE clause in the select_statement is true.

In a WITH CHECK OPTION clause for an updatable view, the LOCAL and CASCADED keywords determine the scope of check testing when the view is defined in terms of another view. The LOCAL keyword restricts the CHECK OPTION only to the view being defined. CASCADED causes the checks for underlying views to be evaluated as well. When neither keyword is given, the default is CASCADED.

For more information about updatable views and the WITH CHECK OPTION clause, see [view-updatability](http://dev.mysql.com/doc/refman/8.0/en/view-updatability.html), and [view-check-option](http://dev.mysql.com/doc/refman/8.0/en/view-check-option.html).

**See also:** : [Online help create-view](http://dev.mysql.com/doc/refman/8.0/en/create-view.html)

### CREATE PROCEDURE Syntax:

These statements create stored routines. By default, a routine is associated with the default database. To associate the routine explicitly with a given database, specify the name as db_name.sp_name when you create it.

The [CREATE FUNCTION](local:CREATE FUNCTION) statement is also used in MySQL to support UDFs (user-defined functions). See [adding-functions](http://dev.mysql.com/doc/refman/8.0/en/adding-functions.html). A UDF can be regarded as an external stored function. Stored functions share their namespace with UDFs. See [function-resolution](http://dev.mysql.com/doc/refman/8.0/en/function-resolution.html), for the rules describing how the server interprets references to different kinds of functions.

To invoke a stored procedure, use the [CALL](local:CALL) statement (see [call](http://dev.mysql.com/doc/refman/8.0/en/call.html)). To invoke a stored function, refer to it in an expression. The function returns a value during expression evaluation.

[CREATE PROCEDURE](local:CREATE PROCEDURE) and [CREATE FUNCTION](local:CREATE FUNCTION) require the CREATE ROUTINE privilege. If the DEFINER clause is present, the privileges required depend on the user value, as discussed in [stored-objects-security](http://dev.mysql.com/doc/refman/8.0/en/stored-objects-security.html). If binary logging is enabled, [CREATE FUNCTION](local:CREATE FUNCTION) might require the SUPER privilege, as discussed in [stored-programs-logging](http://dev.mysql.com/doc/refman/8.0/en/stored-programs-logging.html).

By default, MySQL automatically grants the ALTER ROUTINE and EXECUTE privileges to the routine creator. This behavior can be changed by disabling the automatic_sp_privileges system variable. See [stored-routines-privileges](http://dev.mysql.com/doc/refman/8.0/en/stored-routines-privileges.html).

The DEFINER and SQL SECURITY clauses specify the security context to be used when checking access privileges at routine execution time, as described later in this section.

If the routine name is the same as the name of a built-in SQL function, a syntax error occurs unless you use a space between the name and the following parenthesis when defining the routine or invoking it later. For this reason, avoid using the names of existing SQL functions for your own stored routines.

The IGNORE_SPACE SQL mode applies to built-in functions, not to stored routines. It is always permissible to have spaces after a stored routine name, regardless of whether IGNORE_SPACE is enabled.

The parameter list enclosed within parentheses must always be present. If there are no parameters, an empty parameter list of () should be used. Parameter names are not case sensitive.

Each parameter is an IN parameter by default. To specify otherwise for a parameter, use the keyword OUT or INOUT before the parameter name.

An IN parameter passes a value into a procedure. The procedure might modify the value, but the modification is not visible to the caller when the procedure returns. An OUT parameter passes a value from the procedure back to the caller. Its initial value is NULL within the procedure, and its value is visible to the caller when the procedure returns. An INOUT parameter is initialized by the caller, can be modified by the procedure, and any change made by the procedure is visible to the caller when the procedure returns.

For each OUT or INOUT parameter, pass a user-defined variable in the [CALL](local:CALL) statement that invokes the procedure so that you can obtain its value when the procedure returns. If you are calling the procedure from within another stored procedure or function, you can also pass a routine parameter or local routine variable as an OUT or INOUT parameter. If you are calling the procedure from within a trigger, you can also pass NEW.col_name as an OUT or INOUT parameter.

For information about the effect of unhandled conditions on procedure parameters, see [conditions-and-parameters](http://dev.mysql.com/doc/refman/8.0/en/conditions-and-parameters.html).

Routine parameters cannot be referenced in statements prepared within the routine; see [stored-program-restrictions](http://dev.mysql.com/doc/refman/8.0/en/stored-program-restrictions.html).

The following example shows a simple stored procedure that uses an OUT parameter:

>
mysql> delimiter //

mysql> CREATE PROCEDURE simpleproc (OUT param1 INT)
    -> BEGIN
    ->   SELECT COUNT(*) INTO param1 FROM t;
    -> END//
Query OK, 0 rows affected (0.00 sec)

mysql> delimiter ;

mysql> CALL simpleproc(@a);
Query OK, 0 rows affected (0.00 sec)

mysql> SELECT @a;
+------+
| @a   |
+------+
| 3    |
+------+
1 row in set (0.00 sec)

  

The example uses the mysql client delimiter command to change the statement delimiter from ; to // while the procedure is being defined. This enables the ; delimiter used in the procedure body to be passed through to the server rather than being interpreted by mysql itself. See [stored-programs-defining](http://dev.mysql.com/doc/refman/8.0/en/stored-programs-defining.html).

The RETURNS clause may be specified only for a FUNCTION, for which it is mandatory. It indicates the return type of the function, and the function body must contain a RETURN value statement. If the [RETURN](local:RETURN) statement returns a value of a different type, the value is coerced to the proper type. For example, if a function specifies an ENUM or SET value in the RETURNS clause, but the [RETURN](local:RETURN) statement returns an integer, the value returned from the function is the string for the corresponding ENUM member of set of SET members.

The following example function takes a parameter, performs an operation using an SQL function, and returns the result. In this case, it is unnecessary to use delimiter because the function definition contains no internal ; statement delimiters:

>
mysql> CREATE FUNCTION hello (s CHAR(20))
mysql> RETURNS CHAR(50) DETERMINISTIC
    -> RETURN CONCAT('Hello, ',s,'!');
Query OK, 0 rows affected (0.00 sec)

mysql> SELECT hello('world');
+----------------+
| hello('world') |
+----------------+
| Hello, world!  |
+----------------+
1 row in set (0.00 sec)

  

Parameter types and function return types can be declared to use any valid data type. The COLLATE attribute can be used if preceded by a CHARACTER SET specification.

The routine_body consists of a valid SQL routine statement. This can be a simple statement such as [SELECT](local:SELECT) or [INSERT](local:INSERT), or a compound statement written using BEGIN and END. Compound statements can contain declarations, loops, and other control structure statements. The syntax for these statements is described in [sql-compound-statements](http://dev.mysql.com/doc/refman/8.0/en/sql-compound-statements.html). In practice, stored functions tend to use compound statements, unless the body consists of a single [RETURN](local:RETURN) statement.

MySQL permits routines to contain DDL statements, such as CREATE and DROP. MySQL also permits stored procedures (but not stored functions) to contain SQL transaction statements such as [COMMIT](local:COMMIT). Stored functions may not contain statements that perform explicit or implicit commit or rollback. Support for these statements is not required by the SQL standard, which states that each DBMS vendor may decide whether to permit them.

Statements that return a result set can be used within a stored procedure but not within a stored function. This prohibition includes [SELECT](local:SELECT) statements that do not have an INTO var_list clause and other statements such as [SHOW](local:SHOW), [EXPLAIN](local:EXPLAIN), and [CHECK TABLE](local:CHECK TABLE). For statements that can be determined at function definition time to return a result set, a Not allowed to return a result set from a function error occurs (ER_SP_NO_RETSET). For statements that can be determined only at runtime to return a result set, a PROCEDURE %s can't return a result set in the given context error occurs (ER_SP_BADSELECT).

[USE](local:USE) statements within stored routines are not permitted. When a routine is invoked, an implicit USE db_name is performed (and undone when the routine terminates). The causes the routine to have the given default database while it executes. References to objects in databases other than the routine default database should be qualified with the appropriate database name.

For additional information about statements that are not permitted in stored routines, see [stored-program-restrictions](http://dev.mysql.com/doc/refman/8.0/en/stored-program-restrictions.html).

For information about invoking stored procedures from within programs written in a language that has a MySQL interface, see [call](http://dev.mysql.com/doc/refman/8.0/en/call.html).

MySQL stores the sql_mode system variable setting in effect when a routine is created or altered, and always executes the routine with this setting in force, regardless of the current server SQL mode when the routine begins executing.

The switch from the SQL mode of the invoker to that of the routine occurs after evaluation of arguments and assignment of the resulting values to routine parameters. If you define a routine in strict SQL mode but invoke it in nonstrict mode, assignment of arguments to routine parameters does not take place in strict mode. If you require that expressions passed to a routine be assigned in strict SQL mode, you should invoke the routine with strict mode in effect.

**See also:** : [Online help create-procedure](http://dev.mysql.com/doc/refman/8.0/en/create-procedure.html)

### ALTER TABLE Syntax:

[ALTER TABLE](local:ALTER TABLE) changes the structure of a table. For example, you can add or delete columns, create or destroy indexes, change the type of existing columns, or rename columns or the table itself. You can also change characteristics such as the storage engine used for the table or the table comment.

- To use [ALTER TABLE](local:ALTER TABLE), you need ALTER, CREATE, and INSERT privileges for the table. Renaming a table requires ALTER and DROP on the old table, ALTER, CREATE, and INSERT on the new table.
    
- Following the table name, specify the alterations to be made. If none are given, [ALTER TABLE](local:ALTER TABLE) does nothing.
    
- The syntax for many of the permissible alterations is similar to clauses of the [CREATE TABLE](local:CREATE TABLE) statement. column_definition clauses use the same syntax for ADD and CHANGE as for [CREATE TABLE](local:CREATE
              TABLE). For more information, see [create-table](http://dev.mysql.com/doc/refman/8.0/en/create-table.html).
    
- The word COLUMN is optional and can be omitted, except for RENAME COLUMN (to distinguish a column-renaming operation from the RENAME table-renaming operation).
    
- Multiple ADD, ALTER, DROP, and CHANGE clauses are permitted in a single [ALTER TABLE](local:ALTER
              TABLE) statement, separated by commas. This is a MySQL extension to standard SQL, which permits only one of each clause per [ALTER TABLE](local:ALTER TABLE) statement. For example, to drop multiple columns in a single statement, do this:
    
    >
    ALTER TABLE t2 DROP COLUMN c, DROP COLUMN d;
    
- If a storage engine does not support an attempted [ALTER TABLE](local:ALTER TABLE) operation, a warning may result. Such warnings can be displayed with [SHOW WARNINGS](local:SHOW WARNINGS). See [show-warnings](http://dev.mysql.com/doc/refman/8.0/en/show-warnings.html). For information on troubleshooting [ALTER TABLE](local:ALTER TABLE), see [alter-table-problems](http://dev.mysql.com/doc/refman/8.0/en/alter-table-problems.html).
    
- For information about generated columns, see [alter-table-generated-columns](http://dev.mysql.com/doc/refman/8.0/en/alter-table-generated-columns.html).
    
- For usage examples, see [alter-table-examples](http://dev.mysql.com/doc/refman/8.0/en/alter-table-examples.html).
    
- InnoDB in MySQL 8.0.17 and later supports addition of multi-valued indexes on JSON columns using a key_part specification can take the form (CAST json_path AS type ARRAY). See [create-index-multi-valued](http://dev.mysql.com/doc/refman/8.0/en/create-index-multi-valued.html), for detailed information regarding multi-valued index creation and usage of, as well as restrictions and limitations on multi-valued indexes.
    
- With the mysql_info() C API function, you can find out how many rows were copied by [ALTER TABLE](local:ALTER TABLE). See [mysql-info](http://dev.mysql.com/doc/refman/8.0/en/mysql-info.html).
    

**See also:** : [Online help alter-table](http://dev.mysql.com/doc/refman/8.0/en/alter-table.html)