# Oracle

<https://www.oracle.com/>

Widely used classic enterprise RDBMS databases with good performance, durability and PL/SQL advanced SQL.

Most of this was not retained to be ported and I don't work on Oracle any more to go back and populate this.

<!-- INDEX_START -->

- [Key Points](#key-points)
- [Local Login as Admin](#local-login-as-admin)
- [SQL Scripts](#sql-scripts)
- [SQL Developer IDE](#sql-developer-ide)
  - [Install SQL Developer](#install-sql-developer)
- [Sqlplus Readline Support](#sqlplus-readline-support)
- [Alter User Password](#alter-user-password)
- [Get Table DDL](#get-table-ddl)
- [Investigate table](#investigate-table)
- [Backup Table to adjacent backup table](#backup-table-to-adjacent-backup-table)
- [Space Clean Up](#space-clean-up)
  - [Purge Recyclebin](#purge-recyclebin)
  - [Purge DBA Recyclebin](#purge-dba-recyclebin)
  - [Shrink Table](#shrink-table)
- [Restore table from adjacent backup table](#restore-table-from-adjacent-backup-table)

<!-- INDEX_END -->

## Key Points

- Expensive
- Widely used battle tested enterprise RDBMS
- Well suited to large-scale databases
- Good performance and optimizations
- Good security and encryption
- Cloud - available on major clouds as a managed database
  - eg. [AWS](aws.md) RDS, [GCP](gcp.md) Cloud SQL, [Azure](azure.md) Database
- SQL, and PL/SQL scripting language for querying and managing data
- Oracle Autonomous Database automates tasks like tuning, backups, and patching using machine learning
- High Availability - RAC (Real Application Clusters) and Data Guard offer high availability and disaster recovery
- Clients - SQL*Plus, SQLcl, and SQL Developer for database management and development

| Port | Description       |
|------|-------------------|
| 1521 | Oracle SQL port   |

## Local Login as Admin

This bypasses all authentication and logs you in as the superuser for administer the DB without needing a password.

First `su` to the `oracle` user under which Oracle was installed:

```shell
sudo su - oracle
```

Then as the `oracle` user start the local `sqlplus` client like this:

```shell
sqlplus / as sysdba
```

## SQL Scripts

Scripts for DBA administration and performance engineering:

[HariSekhon/SQL-scripts](https://github.com/HariSekhon/SQL-scripts)

[![Readme Card](https://github-readme-stats.vercel.app/api/pin/?username=HariSekhon&repo=SQL-scripts&theme=ambient_gradient&description_lines_count=3)](https://github.com/HariSekhon/SQL-scripts)

## SQL Developer IDE

<https://www.oracle.com/database/sqldeveloper>

SQL Developer - free and widely used Oracle-specific IDE.

Alternatives:

- Toad for Oracle
- Navicat for Oracle
- generic [SQL Clients](sql.md#sql-clients)

### Install SQL Developer

[Download link](https://www.oracle.com/database/sqldeveloper/technologies/download/)

Quickly from [DevOps-Bash-tools](devops-bash-tools.md):

```shell
install_oracle_sql_developer.sh
```

## Sqlplus Readline Support

Use readline wrapper in front of `sqlplus` to get command history:

```shell
rlwrap sqlplus user/pass@database
```

`rlwrap` does segfault so you may want to stop using it in certain cases, like with logon prompts or when using password
below.

## Alter User Password

```sql
ALTER USER spacewalk IDENTIFIED BY test;
-- or prompts for a new password
-- also allows for chars like ! which aren't liked on the alter user statement
--PASSWORD
```

## Get Table DDL

Without these doesn't give full show create table output:

```sql
SET PAGESIZE 0;
SET LONG 1000;
```

```sql
SELECT dbms_metadata.get_ddl('TABLE', 'myTable', 'mySchema') FROM DUAL;
```

## Investigate table

```sql
SELECT MIN(row_id), MAX(row_id) FROM myTable;
```

## Backup Table to adjacent backup table

Do this before any risky operations or shrinking tables:

```sql
CREATE TABLE mytable_backup AS SELECT * FROM  mytable;
```

## Space Clean Up

- drop temporary and backup tables if you can

<!-- -->

- purge recyclebin and dba recyclebin

<!-- -->

- shrink tables / tablespaces

### Purge Recyclebin

```sql
SHOW RECYCLEBIN;
```

```sql
PURGE RECYCLEBIN;
```

```sql
SHOW RECYCLEBIN;
```

To only purge the recyclebin for a given table:

```sql
PURGE DBA_RECYCLEBIN;
```

### Purge DBA Recyclebin

This is for all user's recyclebins.

Use
[oracle_show_dba_recyclebin.sql](https://github.com/HariSekhon/SQL-scripts/blob/master/oracle_show_dba_recyclebin.sql)
to see the recyclebin contents for all users.

Then purge it:

```sql
PURGE DBA_RECYCLEBIN;
```

Then re-run
[oracle_show_dba_recyclebin.sql](https://github.com/HariSekhon/SQL-scripts/blob/master/oracle_show_dba_recyclebin.sql)
to check.

### Shrink Table

**First [backup the table](#backup-table-to-adjacent-backup-table)** you are going to shrink to an adjacent backup table.

Then `SHRINK SPACE` of the table to reduce space allocated to it by removing unused space from its data blocks
(optimizes storage and improves performance).

`CASCADE` also shrinks dependent objects eg. indexes:

```sql
ALTER TABLE mytable SHRINK SPACE CASCADE;
```

Check the space again by running scripts in [HariSekhon/SQL-scripts](https://github.com/HariSekhon/SQL-scripts).

[Investigate table](#investigate-table) to check it looks ok.

If happy, then drop the backup table:

```sql
DROP TABLE mytable_backup;
```

If not happy, then [Restore table from adjacent backup table](#restore-table-from-adjacent-backup-table).

## Restore table from adjacent backup table

First check you have the backup table `mytable_backup`...
then empty the table to be restored:

```sql
TRUNCATE TABLE mytable;
```

Then restore the rows from the backup table:

```sql
INSERT INTO mytable SELECT * FROM mytable_backup;
```

**Mostly unretained 2005-2009**