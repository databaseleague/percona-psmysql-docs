# Audit Log Filter overview

The Audit Log Filter plugin allows you to monitor, log, and block a connection or query actively executed on the selected server. 

Enabling the plugin produces a log file that contains a record of server activity. The log file has information on connections and databases accessed by that connection. 

The plugin uses the `mysql` system database to store filter and user account data. Set the [`audit_log_database`](audit-log-filter-options.md#audit_log_database) variable at server startup to select a different database.

The [`AUDIT_ADMIN`](audit-log-privileges.md) privilege is required to enable users to manage the Audit Log Filter plugin.

## Privileges

Define the privilege at runtime at the startup of the server. The associated Audit Log Filter privilege can be unavailable if the plugin is not enabled.

### `AUDIT_ADMIN`

This privilege is defined by the `audit_log_filter` plugin and enables the user to configure the plugin.

## Audit Log Filter tables

The Audit Log Filter plugin uses `mysql` system database tables in the `InnoDB` storage engine. These tables store user account data and filter data. When you start to server, change the plugin's database with the `audit_log_filter_database` variable.

The `audit_log_filter` table stores the definitions of the filters and has the following column definitions:

<!DOCTYPE html>
<html>
<head>
    <title>HTML Table Generator</title> 
    <style>
        #demTable {
            width:100%;
            height:100%;
            border:1px solid #b3adad;
            border-collapse:collapse;
            padding:5px;
        }
        #demTable th {
            border:1px solid #b3adad;
            padding:5px;
            background: #f0f0f0;
            color: #313030;
        }
        #demTable td {
            border:1px solid #b3adad;
            text-align:left;
            padding:5px;
            background: #ffffff;
            color: #313030;
        }
    </style>
</head>
<body>
    <table id="demTable">
        <thead>
            <tr>
                <th><div style="color: #333333;background-color: #f5f5f5;font-family: Menlo, Monaco, 'Courier New', monospace;font-weight: normal;font-size: 14px;line-height: 21px;white-space: pre;">Column name</div></th>
                <th><div style="color: #333333;background-color: #f5f5f5;font-family: Menlo, Monaco, 'Courier New', monospace;font-weight: normal;font-size: 14px;line-height: 21px;white-space: pre;">Description</div></th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>&nbsp;NAME</td>
                <td>&nbsp;Name of the filter</td>
            </tr>
            <tr>
                <td>&nbsp;FILTER</td>
                <td>&nbsp;Definition of the filter linked to the name as a JSON value</td>
            </tr>
        </tbody>
    </table>
</body>
</html>

The `audit_log_filter_user` table stores account data and has the following column definitions:

<!DOCTYPE html>
<html>
<head>
    <title>HTML Table Generator</title> 
    <style>
        #demTable {
            width:100%;
            height:100%;
            border:1px solid #b3adad;
            border-collapse:collapse;
            padding:5px;
        }
        #demTable th {
            border:1px solid #b3adad;
            padding:5px;
            background: #f0f0f0;
            color: #313030;
        }
        #demTable td {
            border:1px solid #b3adad;
            text-align:left;
            padding:5px;
            background: #ffffff;
            color: #313030;
        }
    </style>
</head>
<body>
    <table id="demTable">
        <thead>
            <tr>
                <th><div style="color: #333333;background-color: #f5f5f5;font-family: Menlo, Monaco, 'Courier New', monospace;font-weight: normal;font-size: 14px;line-height: 21px;white-space: pre;">Column name</div></th>
                <th><div style="color: #333333;background-color: #f5f5f5;font-family: Menlo, Monaco, 'Courier New', monospace;font-weight: normal;font-size: 14px;line-height: 21px;white-space: pre;">Description</div></th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>USER</td>
                <td>&nbsp;The account name of the user</td>
            </tr>
            <tr>
                <td>&nbsp;HOST</td>
                <td>&nbsp;The account name of the host</td>
            </tr>
            <tr>
                <td>&nbsp;FILTERNAME</td>
                <td>&nbsp;The account filter name</td>
            </tr>
        </tbody>
    </table>
</body>
</html>