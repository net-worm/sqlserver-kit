# SQL Server Name Convention and T-SQL Programming Style
> There are only two hard things in Computer Science: cache invalidation and naming things
> -- <cite>[Phil Karlton](https://www.karlton.org/2017/12/naming-things-hard/)</cite>

[Naming convention](https://en.wikipedia.org/wiki/Naming_convention_(programming)) is a set of rules for choosing the character sequence to be used for identifiers which denote variables, types, functions, and other entities in source code and documentation.

Reasons for using a naming convention (as opposed to allowing programmers to choose any character sequence) include the following:
- To reduce the effort needed to read and understand source code.
- To enable code reviews to focus on more important issues than arguing over syntax and naming standards.
- To enable code quality review tools to focus their reporting mainly on significant issues other than syntax and style preferences.


## Table of Contents
- [SQL Server Object Name Convention](#sql-server-object-name-convention)
- [SQL Server Data Types Recommendation](#data-types-recommendation)
- [T-SQL Programming T-SQL Style](#t-sql-programming-style)
  - [General T-SQL programming style](#general-t-sql-programming-style)
  - [Stored procedures and functions programming style](#programming-style)
  - [Dynamic T-SQL Recommendation](#dynamic-t-sql-recommendation)
- [Reference and useful links](#reference)


## SQL Server Object Name Convention
<a id="sql-server-object-name-convention"></a>

| Object                                   | Code | Notation   | Length | Plural | Prefix | Suffix | Abbreviation | Char Mask    | Example                              |
|------------------------------------------|------| ---------- |-------:|--------|--------|--------|--------------|--------------|--------------------------------------|
| [Database]                               |      | UPPERCASE  |     30 | No     | No     | No     | Yes          | [A-z]        | `MYDATABASE`                         |
| [Schema]                                 |      | lowercase  |     30 | No     | No     | No     | Yes          | [a-z][0-9]   | `myschema`                           |
| [Global Temporary Table]                 |      | PascalCase |    117 | No     | No     | No     | Yes          | ##[A-z][0-9] | `##MyTable`                          |
| [Local Temporary Table]                  |      | PascalCase |    116 | No     | No     | No     | Yes          | #[A-z][0-9]  | `#MyTable`                           |
| [File Table]                             |      | PascalCase |    128 | No     | FT_    | No     | Yes          | [A-z][0-9]   | `FT_MyTable`                         |
| [Memory-optimized SCHEMA_AND_DATA Table] |      | PascalCase |    128 | No     | MT_    | _SD    | Yes          | [A-z][0-9]   | `MT_MyTable_SD`                      |
| [Memory-optimized SCHEMA_ONLY Table]     |      | PascalCase |    128 | No     | MT_    | _SO    | Yes          | [A-z][0-9]   | `MT_MyTable_SO`                      |
| [Temporal Table]                         |      | PascalCase |    128 | No     | No     | _TT    | Yes          | [A-z][0-9]   | `MyTable_TT`                         |
| [Disk-Based Table]                       | U    | PascalCase |    128 | No     | No     | No     | Yes          | [A-z][0-9]   | `MyTable`                            |
| [Disk-Based Wide Table - SPARSE Column]  | U    | PascalCase |    128 | No     | No     | _SPR   | Yes          | [A-z][0-9]   | `MyTable_SPR`                        |
| [Table Column]                           |      | PascalCase |    128 | No     | No     | No     | Yes          | [A-z][0-9]   | `MyColumn`                           |
| [Table Column SPARSE]                    |      | PascalCase |    128 | No     | No     | _SPR   | Yes          | [A-z][0-9]   | `MyColumn_SPR`                       |
| Table Default Values                     | D    | PascalCase |    128 | No     | DF_    | No     | Yes          | [A-z][0-9]   | `DF_MyTable_MyColumn`                |
| Table Check Column Constraint            | C    | PascalCase |    128 | No     | CK_    | No     | Yes          | [A-z][0-9]   | `CK_MyTable_MyColumn`                |
| Table Check Table Constraint             | C    | PascalCase |    128 | No     | CTK_   | No     | Yes          | [A-z][0-9]   | `CTK_MyTable_MyColumn_AnotherColumn` |
| Table Primary Key                        | PK   | PascalCase |    128 | No     | PK_    | No     | Yes          | [A-z][0-9]   | `PK_MyTableID`                       |
| Table Alternative Key                    | UQ   | PascalCase |    128 | No     | AK_    | No     | Yes          | [A-z][0-9]   | `AK_MyTable_MyColumn_AnotherColumn`  |
| Table Foreign Key                        | F    | PascalCase |    128 | No     | FK_    | No     | Yes          | [A-z][0-9]   | `FK_MyTable_ForeignTableID`          |
| Table Clustered Index                    |      | PascalCase |    128 | No     | IXC_   | No     | Yes          | [A-z][0-9]   | `IXC_MyTable_MyColumn_AnotherColumn` |
| Table Non Clustered Index                |      | PascalCase |    128 | No     | IX_    | No     | Yes          | [A-z][0-9]   | `IX_MyTable_MyColumn_AnotherColumn`  |
| [DDL Trigger]                            | TR   | PascalCase |    128 | No     | TR_    | _DDL   | Yes          | [A-z][0-9]   | `TR_LogicalName_DDL`                 |
| [DML Trigger]                            | TR   | PascalCase |    128 | No     | TR_    | _DML   | Yes          | [A-z][0-9]   | `TR_MyTable_LogicalName_DML`         |
| [Logon Trigger]                          | TR   | PascalCase |    128 | No     | TR_    | _LOG   | Yes          | [A-z][0-9]   | `TR_LogicalName_LOG`                 |
| [View]                                   | V    | PascalCase |    128 | No     | VI_    | No     | No           | [A-z][0-9]   | `VI_LogicalName`                     |
| [Indexed View]                           | V    | PascalCase |    128 | No     | VIX_   | No     | No           | [A-z][0-9]   | `VIx_LogicalName`                    |
| [Stored Procedure]                       | P    | PascalCase |    128 | No     | usp_   | No     | No           | [A-z][0-9]   | `usp_LogicalName`                    |
| [Scalar User-Defined Function]           | FN   | PascalCase |    128 | No     | udf_   | No     | No           | [A-z][0-9]   | `udf_FunctionLogicalName`            |
| [Table-Valued Function]                  | FN   | PascalCase |    128 | No     | tvf_   | No     | No           | [A-z][0-9]   | `tvf_FunctionLogicalName`            |
| [Synonym]                                | SN   | camelCase  |    128 | No     | sy_    | No     | No           | [A-z][0-9]   | `sy_logicalName`                     |
| [Sequence]                               | SO   | PascalCase |    128 | No     | sq_    | No     | No           | [A-z][0-9]   | `sq_TableName`                       |
| CLR Assembly                             |      | PascalCase |    128 | No     | CA     | No     | Yes          | [A-z][0-9]   | `CALogicalName`                      |
| CLR Stored Procedures                    | PC   | PascalCase |    128 | No     | pc_    | No     | Yes          | [A-z][0-9]   | `pc_CAName_LogicalName`              |
| CLR Scalar User-Defined Function         |      | PascalCase |    128 | No     | cudf_  | No     | No           | [A-z][0-9]   | `cudf_CAName_LogicalName`            |
| CLR Table-Valued Function                |      | PascalCase |    128 | No     | ctvf_  | No     | No           | [A-z][0-9]   | `ctvf_CAName_LogicalName`            |
| CLR User-Defined Aggregates              |      | PascalCase |    128 | No     | ca_    | No     | No           | [A-z][0-9]   | `ca_CAName_LogicalName`              |
| CLR User-Defined Types                   |      | PascalCase |    128 | No     | ct_    | No     | No           | [A-z][0-9]   | `ct_CAName_LogicalName`              |
| CLR Triggers                             |      | PascalCase |    128 | No     | ctr_   | No     | No           | [A-z][0-9]   | `ctr_CAName_LogicalName`             |

[Database]:https://docs.microsoft.com/en-us/sql/t-sql/statements/create-database-transact-sql
[Schema]:https://docs.microsoft.com/en-us/sql/relational-databases/security/authentication-access/create-a-database-schema
[Global Temporary Table]:https://docs.microsoft.com/en-us/sql/relational-databases/tables/tables
[Local Temporary Table]:https://docs.microsoft.com/en-us/sql/relational-databases/tables/tables
[File Table]:https://docs.microsoft.com/en-us/sql/relational-databases/blob/filetables-sql-server
[Memory-optimized SCHEMA_AND_DATA Table]:https://docs.microsoft.com/en-us/sql/relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables
[Memory-optimized SCHEMA_ONLY Table]:https://docs.microsoft.com/en-us/sql/relational-databases/in-memory-oltp/defining-durability-for-memory-optimized-objects
[Temporal Table]:https://docs.microsoft.com/en-us/sql/relational-databases/tables/temporal-tables
[Disk-Based Table]:https://docs.microsoft.com/en-us/sql/relational-databases/in-memory-oltp/comparing-disk-based-table-storage-to-memory-optimized-table-storage
[Disk-Based Wide Table - SPARSE Column]:https://docs.microsoft.com/en-us/sql/relational-databases/tables/tables#wide-tables
[Table Column]:https://docs.microsoft.com/en-us/sql/t-sql/statements/alter-table-transact-sql
[Table Column SPARSE]:https://docs.microsoft.com/en-us/sql/relational-databases/tables/use-sparse-columns

[DDL Trigger]:https://docs.microsoft.com/en-us/sql/t-sql/statements/create-trigger-transact-sql
[DML Trigger]:https://docs.microsoft.com/en-us/sql/relational-databases/triggers/dml-triggers
[Logon Trigger]:https://docs.microsoft.com/en-us/sql/t-sql/statements/create-trigger-transact-sql
[View]:https://docs.microsoft.com/en-us/sql/relational-databases/views/views
[Indexed View]:https://docs.microsoft.com/en-us/sql/relational-databases/views/create-indexed-views
[Stored Procedure]:https://docs.microsoft.com/en-us/sql/t-sql/statements/create-procedure-transact-sql
[Scalar User-Defined Function]:https://docs.microsoft.com/en-us/sql/relational-databases/user-defined-functions/create-user-defined-functions-database-engine#Scalar
[Table-Valued Function]:https://docs.microsoft.com/en-us/sql/relational-databases/user-defined-functions/create-user-defined-functions-database-engine#TVF
[Synonym]:https://docs.microsoft.com/en-us/sql/relational-databases/synonyms/synonyms-database-engine
[Sequence]:https://docs.microsoft.com/en-us/sql/relational-databases/sequence-numbers/sequence-numbers

**[⬆ back to top](#table-of-contents)**


## SQL Server Data Types Recommendation
<a id="data-types-recommendation"></a>
More details about SQL Server data types and mapping it with another databases and program languages you can find [here](https://github.com/ktaranov/sqlserver-kit/blob/master/SQL%20Server%20Data%20Types.md)

| General Type         | Type                | ANSI | Recommended    | What use instead   | Why use or not                                            |
|----------------------|---------------------|------|----------------|--------------------|-----------------------------------------------------------|
| Exact Numerics       | [bit]               | No   | *Maybe*        | [tinyint][1]       | bit convert any number (except 0) to 1, 0 converted to 0  |
| Exact Numerics       | [tinyint][1]        | No   | *Maybe*        | [int][1]           |                                                           |
| Exact Numerics       | [smallint][1]       | Yes  | *Maybe*        | [int][1]           |                                                           |
| Exact Numerics       | [int][1]            | Yes  | Yes            | -                  |                                                           |
| Exact Numerics       | [bigint][1]         | No   | Yes            | [int][1]           |                                                           |
| Exact Numerics       | [decimal][2]        | Yes  | Yes            | -                  |                                                           |
| Exact Numerics       | [smallmoney][3]     | No   | *Maybe*        | [decimal][2]       | [possibility to loss precision due to rounding errors][9] |
| Exact Numerics       | [money][3]          | No   | *Maybe*        | [decimal][2]       | [possibility to loss precision due to rounding errors][9] |
| Approximate Numerics | [real][4]           | Yes  | Yes            | -                  |                                                           |
| Approximate Numerics | [float][4](1-24)    | Yes  | No             | [real][4]          | sql server automatically converts float(1-24) to real     |
| Approximate Numerics | [float][4](24-53)   | Yes  | Yes            | -                  |                                                           |
| Date and Time        | [date]              | Yes  | Yes            | -                  |                                                           |
| Date and Time        | [smalldatetime]     | No   | *Maybe*        | [date]             |                                                           |
| Date and Time        | [time]              | Yes  | Yes            | -                  |                                                           |
| Date and Time        | [datetime2]         | No   | Yes            | -                  |                                                           |
| Date and Time        | [datetime]          | Yes  | *Maybe*        | [datetime2]        | [On the Advantages of DateTime2(n) over DateTime]         |
| Date and time        | [datetimeoffset]    | No   | Yes            | -                  |                                                           |
| Character Strings    | [char][5]           | Yes  | *Maybe*        | [varchar][5]       | Save 1 byte from varchar, but be ready for trailing spaces|
| Character Strings    | [varchar][5]        | Yes  | Yes            | -                  |                                                           |
| Character Strings    | [varchar(max)][5]   | Yes  | Yes            | -                  |                                                           |
| Character Strings    | [nchar][6]          | Yes  | *Maybe*        | [nvarchar][6]      |                                                           |
| Character Strings    | [nvarchar][6]       | Yes  | Yes            | -                  |                                                           |
| Character Strings    | [nvarchar(max)][6]  | Yes  | Yes            | -                  |                                                           |
| Character Strings    | [ntext][7]          | No   | **Deprecated** | [nvarchar(max)][6] |                                                           |
| Character Strings    | [text][7]           | No   | **Deprecated** | [nvarchar(max)][6] |                                                           |
| Binary Strings       | [image][7]          | No   | **Deprecated** | [nvarchar(max)][6] |                                                           |
| Binary Strings       | [binary][8]         | Yes  | **Deprecated** | [nvarchar(max)][6] |                                                           |
| Binary Strings       | [varbinary][8]      | Yes  | Yes            | -                  |                                                           |
| Binary Strings       | [varbinary(max)][8] | Yes  | *Maybe*        | -                  |                                                           |
| Other Data Types     | [cursor]            | No   | Yes            | -                  |                                                           |
| Other Data Types     | [sql_variant]       | No   | Yes            | -                  |                                                           |
| Other Data Types     | [hierarchyid]       | No   | Yes            | -                  |                                                           |
| Other Data Types     | [rowversion]        | No   | *Maybe*        | -                  |                                                           |
| Other Data Types     | [timestamp]         | No   | **Deprecated** | [rowversion]       | it is just synonym to [rowversion] data type              |
| Other Data Types     | [uniqueidentifier]  | No   | Yes            | -                  |                                                           |
| Other Data Types     | [xml]               | Yes  | Yes            | -                  |                                                           |
| Other Data Types     | [table]             | No   | *Maybe*        | -                  |                                                           |
| Spatial Data Types   | [geometry]          | No   | Yes            | -                  |                                                           |
| Spatial Data Types   | [geography]         | No   | Yes            | -                  |                                                           |

[1]:https://docs.microsoft.com/sql/t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql
[2]:https://docs.microsoft.com/sql/t-sql/data-types/decimal-and-numeric-transact-sql
[3]:https://docs.microsoft.com/sql/t-sql/data-types/money-and-smallmoney-transact-sql
[4]:https://docs.microsoft.com/sql/t-sql/data-types/float-and-real-transact-sql
[5]:https://docs.microsoft.com/sql/t-sql/data-types/char-and-varchar-transact-sql
[6]:https://docs.microsoft.com/sql/t-sql/data-types/nchar-and-nvarchar-transact-sql
[7]:https://docs.microsoft.com/sql/t-sql/data-types/ntext-text-and-image-transact-sql
[8]:https://docs.microsoft.com/sql/t-sql/data-types/binary-and-varbinary-transact-sql

[9]:https://www.red-gate.com/hub/product-learning/sql-prompt/avoid-use-money-smallmoney-datatypes

[bit]:https://docs.microsoft.com/sql/t-sql/data-types/bit-transact-sql
[date]:https://docs.microsoft.com/sql/t-sql/data-types/date-transact-sql
[smalldatetime]:https://docs.microsoft.com/sql/t-sql/data-types/smalldatetime-transact-sql
[time]:https://docs.microsoft.com/sql/t-sql/data-types/time-transact-sql
[datetime2]:https://docs.microsoft.com/sql/t-sql/data-types/datetime2-transact-sql
[datetime]:https://docs.microsoft.com/sql/t-sql/data-types/datetime-transact-sql
[datetimeoffset]:https://docs.microsoft.com/sql/t-sql/data-types/datetimeoffset-transact-sql
[cursor]:https://docs.microsoft.com/sql/t-sql/data-types/cursor-transact-sql
[sql_variant]:https://docs.microsoft.com/sql/t-sql/data-types/sql-variant-transact-sql
[hierarchyid]:https://docs.microsoft.com/sql/t-sql/data-types/hierarchyid-data-type-method-reference
[rowversion]:https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql
[timestamp]:https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql#remarks
[uniqueidentifier]:https://docs.microsoft.com/sql/t-sql/data-types/uniqueidentifier-transact-sql
[xml]:https://docs.microsoft.com/sql/t-sql/xml/xml-transact-sql
[table]:https://docs.microsoft.com/sql/t-sql/data-types/table-transact-sql
[geometry]:https://docs.microsoft.com/sql/t-sql/spatial-geometry/spatial-types-geometry-transact-sql
[geography]:https://docs.microsoft.com/sql/t-sql/spatial-geography/spatial-types-geography
[On the Advantages of DateTime2(n) over DateTime]:http://www.sqltact.com/2012/12/on-advantages-of-datetime2n-over.html

**[⬆ back to top](#table-of-contents)**


## T-SQL Programming Style
<a id="t-sql-programming-style"></a>
SQL Server T-SQL Coding Conventions, Best Practices, and Programming Guidelines.


### General programming T-SQL style
<a id="#general-t-sql-programming-style"></a>

 - For database objects names in code use only schema plus object name, do not hardcode server and database names in your code: `dbo.MyTable` is good and bad `PRODSERVER.PRODDB.dbo.MyTable`.
   More details [here](https://www.red-gate.com/simple-talk/opinion/editorials/why-you-shouldnt-hardcode-the-current-database-name-in-your-views-functions-and-stored-procedures/),
   [here](https://www.sqlserverscience.com/basics/on-default-schemas-and-search-paths/) and [here](https://www.red-gate.com/hub/product-learning/sql-prompt/finding-code-smells-using-sql-prompt-procedures-lack-schema-qualification).
 - Delimiters: **spaces** (not tabs)
 - Avoid using asterisk in select statements `SELECT *`, use explicit column names.
   More details [here](https://www.red-gate.com/hub/product-learning/sql-prompt/finding-code-smells-using-sql-prompt-asterisk-select-list).
 - No square brackets `[]` and [reserved words](https://github.com/ktaranov/sqlserver-kit/blob/master/Scripts/Check_Reserved_Words_For_Object_Names.sql) in object names and alias, use only Latin symbols **`[A-z]`** and numeric **`[0-9]`**.
 - Prefer [ANSI syntax](http://standards.iso.org/ittf/PubliclyAvailableStandards/c053681_ISO_IEC_9075-1_2011.zip) and functions (`CAST` instead `CONVERT`, `COALESE` instead `ISNULL`).
 - All finished expressions should have semicolon `;` at the end. This is ANSI standard and Microsoft announced with the SQL Server 2008 release that semicolon statement terminators will become mandatory in a future version so statement terminators other than semicolons (whitespace) are currently deprecated. This deprecation announcement means that you should always use semicolon terminators in new development.
   More details [here](http://www.dbdelta.com/always-use-semicolon-statement-terminators/).
 - All script files should end with `GO` and line break.
 - Keywords should be in **UPPERCASE**: `SELECT`, `FROM`, `GROUP BY` etc.
 - Data types declaration should be in **lowercase**: `varchar(30)`, `int`, `real`, `nvarchar(max)` etc.
   More details [here](https://www.sentryone.com/blog/aaronbertrand/backtobasics-lower-case-data-types).
 - All system database and tables must be in **lowercase** for properly working for Case Sensitive instance: `master, sys.tables …`.
 - Avoid non-standard column aliases, use, if required, double-quotes for special characters and always `AS` keyword before alias:
   ```sql
   SELECT p.LastName AS "Last Name"
     FROM dbo.Person AS p;
   ```
   More details [here](https://www.red-gate.com/hub/product-learning/sql-prompt/sql-prompt-code-analysis-avoid-non-standard-column-aliases).
   All possible ways using aliases in SQL Server:

   ```tsql
    /* Recommended due to ANSI */
    SELECT SCHEMA_NAME(schema_id) + '.' + "name" AS "Tables" FROM sys.tables;

    /* Not recommended but possible */
    SELECT SCHEMA_NAME(schema_id) + '.' + [name] AS "Tables" FROM sys.tables;
    SELECT Tables   = SCHEMA_NAME(schema_id) + '.' + [name]  FROM sys.tables;
    SELECT "Tables" = SCHEMA_NAME(schema_id) + '.' + [name]  FROM sys.tables;
    SELECT [Tables] = SCHEMA_NAME(schema_id) + '.' + [name]  FROM sys.tables;
    SELECT 'Tables' = SCHEMA_NAME(schema_id) + '.' + [name]  FROM sys.tables;
    SELECT SCHEMA_NAME(schema_id) + '.' + [name] [Tables]    FROM sys.tables;
    SELECT SCHEMA_NAME(schema_id) + '.' + [name] 'Tables'    FROM sys.tables;
    SELECT SCHEMA_NAME(schema_id) + '.' + [name] "Tables"    FROM sys.tables;
    SELECT SCHEMA_NAME(schema_id) + '.' + [name] Tables      FROM sys.tables;
    SELECT SCHEMA_NAME(schema_id) + '.' + [name] AS [Tables] FROM sys.tables;
    SELECT SCHEMA_NAME(schema_id) + '.' + [name] AS 'Tables' FROM sys.tables;
    SELECT SCHEMA_NAME(schema_id) + '.' + [name] AS Tables   FROM sys.tables;
   ```
 - The first argument in `SELECT` expression should be on the next line:
   ```sql
    SELECT
           FirstName
   ```
 - Arguments are divided by line breaks, commas should be placed before an argument:
   ```sql
   SELECT
          FirstName
        , LastName
   ```
 - For SQL Server >= 2012 use `FETCH-OFFSET` instead `TOP`.
   But if you use `TOP` avoid use `TOP` in a `SELECT` statement without an `ORDER BY`.
   More details [here](https://www.red-gate.com/hub/product-learning/sql-prompt/finding-code-smells-using-sql-prompt-top-without-order-select-statement).
 - Use `TOP` function with brackets because `TOP` has supports use of an expression, such as `(@Rows*2)`, or a subquery: `SELECT TOP(100) LastName …`.
   More details [here](https://www.red-gate.com/hub/product-learning/sql-prompt/sql-prompt-code-analysis-avoiding-old-style-top-clause). Also `TOP` without brackets does not work with `UPDATE` and `DELETE` statements.

   ```tsql
   /* Not working without brackets () */
   DECLARE @n int = 1;
   SELECT TOP@n name FROM sys.objects;
   ```
 - For demo queries use `TOP(100)` or lower value because SQL Server uses one sorting method for `TOP` 1-100 rows, and a different one for 101+ rows.
   More details [here](https://www.brentozar.com/archive/2017/09/much-can-one-row-change-query-plan-part-2/).
 - Avoid using [`ISNUMERIC`](https://docs.microsoft.com/en-us/sql/t-sql/functions/isnumeric-transact-sql) function. Use for SQL Server >= 2012 [`TRY_CONVERT`](https://docs.microsoft.com/en-us/sql/t-sql/functions/try-convert-transact-sql) function and for SQL Server < 2012 `LIKE` expression:
   ```tsql
   CASE WHEN STUFF(LTRIM(TapAngle),1,1,'') NOT LIKE '%[^-+.ED0123456789]%' /* is it a float? */
              AND LEFT(LTRIM(TapAngle),1) LIKE '[-.+0123456789]'
                 AND TapAngle LIKE '%[0123456789][ED][-+0123456789]%'
                 AND RIGHT(TapAngle ,1) LIKE N'[0123456789]'
               THEN 'float'
         WHEN STUFF(LTRIM(TapAngle),1,1,'') NOT LIKE '%[^.0123456789]%' /* is it numeric */
              AND LEFT(LTRIM(TapAngle),1) LIKE '[-.+0123456789]'
              AND TapAngle LIKE '%.%' AND TapAngle NOT LIKE '%.%.%'
              AND TapAngle LIKE '%[0123456789]%'
             THEN 'float'
   ELSE NULL
   END
   ```
   More details [here](https://www.red-gate.com/hub/product-learning/sql-prompt/sql-prompt-code-analysis-avoid-using-isnumeric-function-e1029).
 - Avoid using `INSERT INTO` a permanent table with `ORDER BY`.
   More details [here](https://www.red-gate.com/hub/product-learning/sql-prompt/sql-prompt-code-analysis-insert-permanent-table-order-pe020).
 - Avoid using shorthand (`wk, yyyy, d` etc.) with date/time operations, use full names: `month, day, year`.
   More details [here](https://sqlblog.org/2011/09/20/bad-habits-to-kick-using-shorthand-with-date-time-operations).
 - Avoid ambiguous formats for date-only literals, use `CAST('yyyymmdd' AS DATE)` format.
 - Avoid treating dates like strings and avoid calculations on the left-hand side of the `WHERE` clause.
   More details [here](https://sqlblog.org/2009/10/16/bad-habits-to-kick-mis-handling-date-range-queries).
 - Avoid using [hints](https://docs.microsoft.com/en-us/sql/t-sql/queries/hints-transact-sql) except `RECOMPILE` if needed and `NOEXPAND` (see next tip).
   More details [here](https://www.red-gate.com/hub/product-learning/sql-prompt/sql-prompt-code-analysis-a-hint-is-used-pe004-7).
 - Use `NOEXPAND` hint for [indexed views](https://docs.microsoft.com/sql/relational-databases/views/create-indexed-views) on non enterprise editions of SQL Server to let the query optimizer know that we have indexes.
   More details [here](https://bornsql.ca/blog/using-indexed-views-dont-forget-this-important-tip/).
 - Avoid use of `SELECT…INTO` for production code, use instead `CREATE TABLE` + `INSERT INTO …` approach. More details [here](https://www.red-gate.com/hub/product-learning/sql-prompt/use-selectinto-statement).
 - Use only ISO standard JOINS syntaxes. The *old style* Microsoft/Sybase `JOIN` style for SQL, which uses the `=*` and `*=` syntax, has been deprecated and is no longer used.
   Queries that use this syntax will fail when the database engine level is 10 (SQL Server 2008) or later (compatibility level 100). The ANSI-89 table citation list (`FROM tableA, tableB`) is still ISO standard for `INNER JOINs` only. Neither of these styles are worth using.
   It is always better to specify the type of join you require` INNER`, `LEFT OUTER`, `RIGHT OUTER`, `FULL OUTER` and `CROSS`, which has been standard since ANSI SQL-92 was published. While you can choose any supported `JOIN `style, without affecting the query plan used by SQL Server, using the ANSI-standard syntax will make your code easier to understand, more consistent, and portable to other relational database systems.
   More details [here](https://www.red-gate.com/hub/product-learning/sql-prompt/finding-code-smells-using-sql-prompt-old-style-join-syntax-st001).
 - Do not use a scalar user-defined function (UDF) in a `JOIN` condition, `WHERE` search condition, or in a `SELECT` list, unless the function is [schema-bound](https://docs.microsoft.com/en-us/sql/t-sql/statements/create-function-transact-sql#best-practices).
   More details [here](https://www.red-gate.com/hub/product-learning/sql-prompt/misuse-scalar-user-defined-function-constant-pe017).
 - Use `EXISTS` or `NOT EXISTS` if referencing a subquery, and `IN` or `NOT IN` when have a list of literal values.
   More details [here](https://www.brentozar.com/archive/2018/08/a-common-query-error/).
 - For concatenate unicode strings:
   - always using the upper-case `N`;
   - always store into a variable of type `nvarchar(max)`;
   - avoid truncation of string literals, simply ensure that one piece is converted to `nvarchar(max)`.
   Example:
   ```tsql
   DECLARE @nvcmaxVariable nvarchar(max);
   SET @nvcmaxVariable = CAST(N'ಠ russian anomaly ЯЁЪ ಠ ' AS nvarchar(max)) + N'something else' + N'another';
   SELECT @nvcmaxVariable;
   ```
   More details [here](https://themondaymorningdba.wordpress.com/2018/09/13/them-concatenatin-blues/).
 - Always specify a length to any text-based data type such as `varchar`, `nvarchar`, `char`, `nchar`:
   ```tsql
    /* Correct */
    DECLARE @myGoodVarchareVariable  varchar(50);
    DECLARE @myGoodNVarchareVariable nvarchar(90);
    DECLARE @myGoodCharVariable      char(7);
    DECLARE @myGoodNCharVariable     nchar(10);
    
    /* Not correct */
    DECLARE @myBadVarcharVariable  varchar;
    DECLARE @myBadNVarcharVariable nvarchar;
    DECLARE @myBadCharVariable     char;
    DECLARE @myBadNCharVariable    nchar;
    ```
    More details [here](https://www.red-gate.com/hub/product-learning/sql-prompt/using-a-variable-length-datatype-without-explicit-length-the-whys-and-wherefores).
 - `FROM, WHERE, INTO, JOIN, GROUP BY, ORDER BY` expressions should be aligned so, that all their arguments are placed under each other (see Example below)

Example:

```tsql
WITH CTE_MyCTE AS (
    SELECT      t1.Value1  AS Val1
              , t1.Value2  AS Val2
              , t2.Value3  AS Val3
     INNER JOIN dbo.Table3 AS t2
             ON t1.Value1 = t2.Value1
     WHERE      t1.Value1 > 1
       AND      t2.Value2 >= 101
)
SELECT t1.Value1 AS Val1
     , t1.Value2 AS Val2
     , t2.Value3 AS Val3
INTO   #Table3
FROM   CTE_MyCTE AS t1
ORDER BY t2.Value2;
```

**[⬆ back to top](#table-of-contents)**


### Stored procedures and functions programming style
<a id="programming-style"></a>

 - All stored procedures and functions should use `ALTER` statement and start with the object presence check (see example below)
 - `ALTER` statement should be preceded by 2 line breaks
 - Parameters name should be in **camelCase**
 - Parameters should be placed under procedure name divided by line breaks
 - After the `ALTER` statement and before `AS` keyword should be placed a comment with execution example
 - The procedure or function should begin with parameters checks (see example below)
 - Create `sp_` procedures only in `master` database - SQL Server will always scan through the system catalog first
 - Always use `BEGIN TRY` and `BEGIN CATCH` for error handling
 - Always use multi-line comment `/* */` instead in-line comment `--`
 - Use `SET NOCOUNT ON;` for stops the message that shows the count of the number of rows affected by a Transact-SQL statement and decreasing network traffic.
   More details [here](https://www.red-gate.com/hub/product-learning/sql-prompt/finding-code-smells-using-sql-prompt-set-nocount-problem-pe008-pe009).
 - Do not use `SET NOCOUNT OFF;` because it is default behavior
 - Use `RAISERROR` instead `PRINT` if you want to give feedback about the state of the currently executing SQL batch without lags.
   More details [here](http://sqlity.net/en/984/print-vs-raiserror/) and [here](http://sqlservercode.blogspot.com/2019/01/print-disruptor-of-batch-deletes-in-sql.html).
 - All code should be self documenting
 - T-SQL code, triggers, stored procedures, functions, should have a standard comment-documentation banner:
```tsql
summary:   >
 This procedure returns an object build script as a single-row, single column
 result.
Revisions: 
 - Author: Bill Gates
   Version: 1.1
   Modification: dealt properly with heaps
   date: 2017-07-15
 - version: 1.2
   modification: Removed several bugs and got column-level constraints working
   date: 2017-06-30
example:
     - code: udf_MyFunction 'testValsue';
returns:   >
 single row, single column result Build_Script.
```

**[⬆ back to top](#table-of-contents)**

Stored Procedure Example:

```sql
IF OBJECT_ID('dbo.usp_StoredProcedure', 'P') IS NULL
EXECUTE('CREATE PROCEDURE dbo.usp_StoredProcedure as SELECT 1');
GO


ALTER PROCEDURE dbo.usp_StoredProcedure (
                @parameterValue1 SMALLINT
              , @parameterValue2 NVARCHAR(300)
              , @debug           BIT           = 0
)
/*
EXECUTE dbo.usp_StoredProcedure
        @parameterValue1 = 0
      , @parameterValue2 = N'BULK'
*/
AS
SET NOCOUNT ON;

BEGIN TRY
    IF (@parameterValue1 < 0 OR @parameterValue2 NOT IN ('SIMPLE', 'BULK', 'FULL'))
    RAISERROR('Not valid data parameter!', 16, 1);
    PRINT @parameterValue2;
END TRY

BEGIN CATCH
    /* Print error information. */
    PRINT 'Error: '       + CONVERT(varchar(50), ERROR_NUMBER())   +
          ', Severity: '  + CONVERT(varchar(5),  ERROR_SEVERITY()) +
          ', State: '     + CONVERT(varchar(5),  ERROR_STATE())    +
          ', Procedure: ' + ISNULL(ERROR_PROCEDURE(), '-')         +
          ', Line: '      + CONVERT(varchar(5),  ERROR_LINE())     +
          ', User name: ' + CONVERT(sysname,     CURRENT_USER);
    PRINT ERROR_MESSAGE();
END CATCH;

GO

```

**[⬆ back to top](#table-of-contents)**


### Dynamic T-SQL Recommendation
<a id="dynamic-t-sql-recommendation"></a>
**Highly recommended to read awesome detailed article about dynamic T-SQL by Erland Sommarskog: [The Curse and Blessings of Dynamic SQL](http://sommarskog.se/dynamic_sql.html)**

Dynamic SQL is a programming technique that allows you to construct SQL statements dynamically at runtime.
It allows you to create more general purpose and flexible SQL statement because the full text of the SQL statements may be unknown at compilation.
For example, you can use the dynamic SQL to create a stored procedure that queries data against a table whose name is not known until runtime.

More details [here](http://www.sqlservertutorial.net/sql-server-stored-procedures/sql-server-dynamic-sql/).

- Do not use [nvarchar(max)][6] for your object’s name parameter, use [sysname](https://docs.microsoft.com/en-us/previous-versions/sql/sql-server-2008-r2/ms191240(v=sql.105)?redirectedfrom=MSDN) instead (synonym for nvarchar(128) except that, by default, sysname is NOT NULL).
  ```tsql
  /* Bad */
  DECLARE @tableName nvarchar(max) = N'MyTableName';

  /* Good */
  DECLARE @tableName sysname = N'MyTableName';
  ```
- Do quote the names of your objects properly.
  ```tsql
  /* Bad */
  DECLARE @tsql      nvarchar(max);
  DECLARE @tableName sysname = N'My badly named table!';
  SET @tsql = N'SELECT object_id FROM ' + @tableName;

  /* Good */
  DECLARE @tsql      nvarchar(max);
  DECLARE @tableName sysname = N'My badly named table 111!';
  SET @tsql = N'SELECT object_id FROM ' + QUOTENAME(@tableName);
  ```
- Always use [`sp_executesql`] instead [`EXEC`] to prevent sql injection.
  Also [`sp_executesql`] can parameterizing your dynamic statement that means plans can be reused as well (when the value of the dynamic object is the same).
  Also [`sp_executesql`] can even be used to output values as well (see example below).
  ```tsql
  /* Bad EXEC example with sql injection*/
  DECLARE @tsql      nvarchar(max);
  DECLARE @tableName sysname = N'master.sys.tables; SELECT * FROM master.sys.server_principals;';
  SET @tsql = N'SELECT "name" FROM ' + @tableName + N';';
  EXEC (@tsql);

  /* Good sp_executesql example*/
  DECLARE @tsql      nvarchar(max);
  DECLARE @tableName sysname = N'master.sys.tables';
  DECLARE @id        int     = 2107154552;
  SET @tsql = N'SELECT name FROM '   + @tableName +
              N' WHERE object_id = ' + CONVERT(nvarchar(max), @id);
  EXEC sp_executesql @tsql, N'@ID int', @ID = @id;

  /* Good sp_executesql example with OUTPUT */
  DECLARE @tsql      nvarchar(max);
  DECLARE @tableName sysname = N'master.sys.tables';
  DECLARE @count     bigint;
  SET @tsql = N'SELECT @countOUT = COUNT(*) FROM ' + @tableName + N';';
  EXEC sp_executesql @tsql, N'@countOUT bigint OUTPUT', @countOUT = @count OUTPUT;
  PRINT('@count = ' + CASE WHEN @count IS NULL THEN 'NULL' ELSE CAST(@count AS varchar(30)) END);
  ```
- Do not use dynamic T-SQL if your statement is not dynamic.
  ```tsql
  /* Bad */
  DECLARE @tsql nvarchar(max);
  DECLARE @id   int = 2107154552;
  SET @tsql = N'SELECT object_id, "name" FROM master.sys.tables WHERE object_id = ' + CONVERT(nvarchar(max), @id);
  EXEC sp_executesql @tsql;

  /* Good */
  DECLARE @id int = 2107154552;
  SELECT object_id, "name" FROM master.sys.tables WHERE object_id = @id;
  ```
- Do not debug the code that creates the dynamic T-SQL first, debug the generated T-SQL statement instead.
  Use `@debug` variable to print (or a `SELECT` statement if your dynamic T-SQL is over 4000 characters) dynamic statement instead executing it.
- Do take the time to format your dynamic T-SQL.
  ```tsql
  /* Bad @tsql formating */
  DECLARE @tsql  nvarchar(max);
  DECLARE @sep   nvarchar(30) = ' UNION ALL ';
  DECLARE @debug bit          = 1;

  SELECT @tsql = COALESCE(@tsql, N'') +
                 N'SELECT N' + QUOTENAME(name,'''') +
                 N' AS DBName, (SELECT COUNT(*) FROM ' +
                 QUOTENAME(name) + N'.sys.tables) AS TableCount' +
                 @sep
  FROM sys.databases
  ORDER BY name;

  SET @tsql = LEFT(@tsql, LEN(@tsql) - LEN(@sep));
  IF @debug = 1 SELECT @tsql AS "tsql" ELSE EXEC sp_executesql @tsql;

  /* Good @tsql formating */
  DECLARE @tsql  nvarchar(max);
  DECLARE @sep   nvarchar(30) = ' UNION ALL ';
  DECLARE @debug bit          = 1;
  DECLARE @crlf  nvarchar(10) = NCHAR(13) + NCHAR(10);

  SELECT @tsql = COALESCE(@tsql, N'') + @crlf +
                 N'SELECT N' + QUOTENAME(name,'''') + N' AS DBName' + @crlf +
                 N'     , (SELECT COUNT(*) FROM ' + QUOTENAME(name) + N'.sys.tables) AS TableCount' + @crlf +
                 @sep
  FROM sys.databases
  ORDER BY name;

  SET @tsql = LEFT(@tsql, LEN(@tsql) - LEN(@sep)) + N';';
  IF @debug = 1 SELECT @tsql AS "tsql" ELSE EXEC sp_executesql @tsql;
  ```


## Official Reference and useful links
<a id="reference"></a>
 - [Transact-SQL Formatting Standards](https://www.simple-talk.com/sql/t-sql-programming/transact-sql-formatting-standards-%28coding-styles%29/) (by Robert Sheldon)
 - [Subjectivity: Naming Standards](http://blogs.sqlsentry.com/aaronbertrand/subjectivity-naming-standards/) (by Aaron Bertrand)
 - [General Database Conventions](http://kejser.org/database-naming-conventions/general-database-conventions/) (by Thomas Kejser)
 - [Writing Readable SQL](http://www.codeproject.com/Articles/126380/Writing-Readable-SQL) (by Red Gate)
 - [SQL Style Guide](http://www.sqlstyle.guide/) (by Simon Holywell)
 - [SQL Code Layout and Beautification](https://www.simple-talk.com/sql/t-sql-programming/sql-code-layout-and-beautification/) (by William Brewer)
 - [TSQL Coding Style](http://www.databasejournal.com/features/mssql/tsql-coding-style.html) (by Gregory Larsen)
 - [User-Defined Functions](https://docs.microsoft.com/en-us/sql/relational-databases/user-defined-functions/user-defined-functions)
 - [Synonyms (Database Engine)](https://docs.microsoft.com/en-us/sql/relational-databases/synonyms/synonyms-database-engine)
 - [Primary and Foreign Key Constraints](https://docs.microsoft.com/en-us/sql/relational-databases/tables/primary-and-foreign-key-constraints)
 - [sys.objects](https://docs.microsoft.com/en-us/sql/relational-databases/system-catalog-views/sys-objects-transact-sql)
 - [SQL Server Constraints](https://docs.microsoft.com/en-us/sql/t-sql/statements/alter-table-table-constraint-transact-sql)
 - [CHECK Constraint TECHNET](http://technet.microsoft.com/en-us/library/ms188258%28v=sql.105%29.aspx)
 - [SQL Server CLR Integration](https://docs.microsoft.com/en-us/dotnet/framework/data/adonet/sql/introduction-to-sql-server-clr-integration)
 - [Deploying CLR Database Objects](https://docs.microsoft.com/en-us/sql/relational-databases/clr-integration/deploying-clr-database-objects)
 - [CLR Stored Procedures](https://docs.microsoft.com/en-us/sql/database-engine/dev-guide/clr-stored-procedures)
 - [User-defined Functions](https://docs.microsoft.com/en-us/sql/relational-databases/user-defined-functions/create-user-defined-functions-database-engine)
 - [SET NOCOUNT ON (Transact-SQL)](https://docs.microsoft.com/en-us/sql/t-sql/statements/set-nocount-transact-sql)
 - [T-SQL Coding Guidelines Presentation](http://www.slideshare.net/chris1adkin/t-sql-coding-guidelines) (by Chris Adkin)
 - [Sql Coding Style](http://c2.com/cgi/wiki?SqlCodingStyle)
 - [SQL Server Code Review Checklist for Developers](https://www.sqlshack.com/sql-server-code-review-checklist-for-developers/) (by Samir Behara)
 - [SQL Formatting standards – Capitalization, Indentation, Comments, Parenthesis](https://solutioncenter.apexsql.com/sql-formatting-standards-capitalization-indentation-comments-parenthesis/) (by ApexSQL)
 - [In The Cloud: The Importance of Being Organized](http://sqlblog.com/blogs/john_paul_cook/archive/2017/05/16/in-the-cloud-the-importance-of-being-organized.aspx)
 - [Naming Conventions in Azure](http://www.sqlchick.com/entries/2017/6/24/naming-conventions-in-azure)
 - [The Basics of Good T-SQL Coding Style – Part 3: Querying and Manipulating Data](https://www.simple-talk.com/sql/t-sql-programming/basics-good-t-sql-coding-style-part-3-querying-manipulating-data/)
 - [SQL naming conventions](https://www.red-gate.com/simple-talk/blogs/sql-naming-conventions/) (by Phi Factor)
 - [SQL Server Compact Object Limitations](http://technet.microsoft.com/en-us/library/ms172451%28v=sql.110%29.aspx)
 - [Dos and Don'ts of Dynamic SQL](https://www.sqlservercentral.com/articles/dos-and-donts-of-dynamic-sql) (by Thom Andrews)

**[⬆ back to top](#table-of-contents)**

[`sp_executesql`]:https://docs.microsoft.com/en-us/sql/relational-databases/system-stored-procedures/sp-executesql-transact-sql
[`EXEC`]:https://docs.microsoft.com/en-us/sql/t-sql/language-elements/execute-transact-sql
