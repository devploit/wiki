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
      <td style="text-align:left"><code>SELECT version()</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Comments</td>
      <td style="text-align:left">
        <p><code>SELECT 1; --comment</code>
        </p>
        <p><code>SELECT /*comment*/1;</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Current User</td>
      <td style="text-align:left">
        <p><code>SELECT user;</code>
        </p>
        <p><code>SELECT current_user;</code>
        </p>
        <p><code>SELECT session_user;</code>
        </p>
        <p><code>SELECT getpgusername();</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">List Users</td>
      <td style="text-align:left"><code>SELECT usename FROM pg_user</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">List Password Hashes</td>
      <td style="text-align:left"><code>SELECT usename, passwd FROM pg_shadow</code><em><code> -- [priv]</code></em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">List Privileges</td>
      <td style="text-align:left"><code>SELECT usename, usecreatedb, usesuper, usecatupd FROM pg_user</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">List DBA Accounts</td>
      <td style="text-align:left"><code>SELECT usename FROM pg_user WHERE usesuper IS TRUE</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Check if Current User is Superuser</td>
      <td style="text-align:left"><code>SELECT current_setting(&apos;is_superuser&apos;)=&apos;on&apos;</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">List Databases</td>
      <td style="text-align:left"><code>SELECT datname FROM pg_database</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">List Tables</td>
      <td style="text-align:left"><code>SELECT c.relname FROM pg_catalog.pg_class c LEFT JOIN pg_catalog.pg_namespace n ON n.oid = c.relnamespace WHERE c.relkind IN (&apos;r&apos;,&apos;&apos;) AND n.nspname NOT IN (&apos;pg_catalog&apos;, &apos;pg_toast&apos;) AND pg_catalog.pg_table_is_visible(c.oid)</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">List Columns</td>
      <td style="text-align:left"><code>SELECT relname, A.attname FROM pg_class C, pg_namespace N, pg_attribute A, pg_type T WHERE (C.relkind=&apos;r&apos;) AND (N.oid=C.relnamespace) AND (A.attrelid=C.oid) AND (A.atttypid=T.oid) AND (A.attnum&gt;0) AND (NOT A.attisdropped) AND (N.nspname ILIKE &apos;public&apos;)</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Find Tables from Column Name</td>
      <td style="text-align:left">
        <p><em><code>If you want to list all the table names that contain a column LIKE &apos;%password%&apos;:</code></em>
        </p>
        <p><code>SELECT DISTINCT relname FROM pg_class C, pg_namespace N, pg_attribute A, pg_type T WHERE (C.relkind=&apos;r&apos;) AND (N.oid=C.relnamespace) AND (A.attrelid=C.oid) AND (A.atttypid=T.oid) AND (A.attnum&gt;0) AND (NOT A.attisdropped) AND (N.nspname ILIKE &apos;public&apos;) AND attname LIKE &apos;%password%&apos;;</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Current Database</td>
      <td style="text-align:left"><code>SELECT current_database()</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Hostname, IP Address</td>
      <td style="text-align:left">
        <p><code>SELECT inet_server_addr();</code><em><code> &#x2014; returns db server IP address (or null if using local connection) </code></em>
        </p>
        <p><code>SELECT inet_server_port();</code><em><code> &#x2014; returns db server IP address (or null if using local connection)</code></em>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Create Users</td>
      <td style="text-align:left">
        <p><code>CREATE USER test1 PASSWORD &apos;pass1&apos;;</code><em><code> -- [priv]</code></em>
        </p>
        <p><code>CREATE USER test1 PASSWORD &apos;pass1&apos; CREATEUSER; </code><em><code>-- [priv]  </code></em><code>#grant some privs at the same time</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Delete Users</td>
      <td style="text-align:left"><code>DROP USER test1; </code><em><code>-- [priv]</code></em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Make User DBA</td>
      <td style="text-align:left"><code>ALTER USER test1 CREATEUSER CREATEDB; </code><em><code>-- [priv]</code></em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Location of DB Files</td>
      <td style="text-align:left">
        <p><code>SELECT current_setting(&apos;data_directory&apos;); </code><em><code>-- [priv]</code></em>
        </p>
        <p><code>SELECT current_setting(&apos;hba_file&apos;); </code><em><code>-- [priv]</code></em>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Reading Files</td>
      <td style="text-align:left">
        <p><code>COPY hax0r from $$c:\hax0r.txt$$;</code><em><code> --[priv]</code></em>
        </p>
        <p><code>SELECT content from hax0r;</code><em><code> --[priv]</code></em>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Writing Files</td>
      <td style="text-align:left">
        <p><code>CREATE temp table hax0r (content text); </code><em><code>-- [priv]</code></em>
        </p>
        <p><code>COPY (SELECT $$hax0r$$) to $$c:\hax0r.txt$$;</code><em><code> -- [priv]</code></em>
        </p>
      </td>
    </tr>
  </tbody>
</table>



