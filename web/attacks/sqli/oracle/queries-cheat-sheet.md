# Queries Cheat Sheet

Some of the queries in the table below can only be run by an admin. These are marked with "_-- \[priv\]"_ at the end of the query.

| Usage | Query |
| :--- | :--- |
| Version | `SELECT banner FROM v$version WHERE banner LIKE ‘Oracle%’; SELECT banner FROM v$version WHERE banner LIKE ‘TNS%’; SELECT version FROM v$instance;` |
| Comments | `SELECT 1 -- comment` |
| Current User | `SELECT user FROM dual` |
| List Users | `SELECT username FROM all_users ORDER BY username;` `SELECT name FROM sys.user$;` _`-- [priv]`_ |
| List Password Hashes | `SELECT name, password, astatus FROM sys.user$` _`-- [priv] <= 10g. astatus tells you if acct is locked`_ `SELECT name,spare4 FROM sys.user$`_`-- [priv] # 11g`_ |
| List Privileges | `SELECT`  _`FROM session_privs; -- [priv]`_  `SELECT FROM dba_sys_privs WHERE grantee = ‘DBSNMP’;` _`-- [priv]`_`# list a user’s privs` `SELECT grantee FROM dba_sys_privs WHERE privilege = ‘SELECT ANY DICTIONARY’;`_`-- [priv] # find users with a particular priv`_ `SELECT GRANTEE, GRANTED_ROLE FROM DBA_ROLE_PRIVS;` |
| List DBA Accounts | `SELECT DISTINCT grantee FROM dba_sys_privs WHERE ADMIN_OPTION = ‘YES’;` _`-- [priv] priv, list DBAs, DBA roles`_ |
| Current Database | `SELECT global_name FROM global_name; SELECT name FROM v$database; SELECT instance_name FROM v$instance; SELECT SYS.DATABASE_NAME FROM DUAL;` |
| List Databases | `SELECT DISTINCT owner FROM all_tables;` _`# list schemas (one per user)`_ |
| List Tables | `SELECT table_name FROM all_tables; SELECT owner, table_name FROM all_tables;` |
| List Columns | `SELECT column_name FROM all_tab_columns WHERE table_name = ‘blah’; SELECT column_name FROM all_tab_columns WHERE table_name = ‘blah’ and owner = ‘foo’;` |
| Find Tables from Column Name | `SELECT owner, table_name FROM all_tab_columns WHERE column_name LIKE ‘%PASS%’;` _`# NB: table names are upper case`_ |
| Hostname, IP Address | `SELECT UTL_INADDR.get_host_name FROM dual;` `SELECT host_name FROM v$instance;` `SELECT UTL_INADDR.get_host_address FROM dual;` _`# gets IP address`_ `SELECT UTL_INADDR.get_host_name(’10.0.0.1′) FROM dual;` _`# gets hostnames`_ |
| Location of DB Files | `SELECT name FROM V$DATAFILE;` |
| Get all tablenames in one string | `SELECT rtrim(xmlagg(xmlelement(e, table_name || ‘,’)).extract(‘//text()’).extract(‘//text()’) ,’,') from all_tables` _`# when using union based SQLI with only one row`_ |

