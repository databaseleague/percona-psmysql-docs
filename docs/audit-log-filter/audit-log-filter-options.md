# Audit Log Filter options, variables, and functions

## Options

If you specify a value that is incorrect at server startup, the plugin may fail and not be loaded by the server. If the plugin fails to load, the server may generate error messages for other audit log settings.

The following list are the available variables for the Audit Log filter feature:

audit_log_filter_handler
audit_log_filter_syslog_ident
audit_log_filter_syslog_facility
audit_log_filter_syslog_priority

### --audit-log-filter

| Option name | Description |
| --- | --- |
| Command-line | --audit-log-filter |
| Dynamic | Yes  |
| Scope | Global  |
| Data type | Enumeration  |
| Default | ON  |

Defines how the server loads the plugin. You must have registered or loaded the plugin using either `INSTALL_PLUGIN` or `--plugin-load` or `--plugin-load-add`. 

The value can only be a plugin-loading operation. Enabling the plugin exposes other options and status variables that control logging.

The valid values are the following:

| Values |
| --- |
| ON |
| OFF |
| FORCE |
| FORCE_PLUS_PERMANENT |

### audit_log_filter_buffer_size

| Option name | Description |
| --- | --- |
| Command-line | --audit-log-filter-buffer-size |
| Dynamic | No  |
| Scope | Global  |
| Data type | Integer  |
| Default | 1048576  |
| Minimum value | 4096 |
| Maximum value | 18446744073709547520 |
| Units | byes |
| Block size | 4096 |

Defines the buffer size in multiples of 4096 when logging is asynchronous. The contents for events are stored in a buffer. The contents are stored until the contents are written.

The plugin initializes a single buffer and removes the buffer when the plugin terminates.

### audit_log_filter_compression

| Option name | Description |
| --- | --- |
| Command-line | --audit-log-filter-compression |
| Dynamic | Yes  |
| Scope | Global  |
| Data type | Enumeration  |
| Default | NONE  |
| Valid values | NONE or GZIP |

Defines the compression type for the audit log filter file. The values can be either `NONE`, the default value and no compression) or `GZIP`.


### audit_log_filter_database

| Option name | Description |
| --- | --- |
| Command-line | --audit-log-filter-database |
| Dynamic | No  |
| Scope | Global  |
| Data type | String  |
| Default | mysql  |

Defines the audit_log_filter database, used to store the necessary tables, and is read-only. Set this option at system startup. The database name cannot exceed 64 characters or be `NULL`.

Using an invalid database name prevents the use of audit log filter tables.

### audit_log_filter_disable

| Option name | Description |
| --- | --- |
| Command-line | --audit-log-filter-disable |
| Dynamic | Yes  |
| Scope | Global  |
| Data type | Boolean  |
| Default | OFF |

Disables logging for the plugin for all connections and any sessions. 

Using this variable requires the user account to have `SYSTEM_VARIABLES_ADMIN` and `AUDIT_ADMIN` privileges.

### audit_log_filter_encryption

| Option name | Description |
| --- | --- | 
| Command-line | --audit-log-filter-encryption |
| Dynamic | No  |
| Scope | Global  |
| Data type | Enumeration  |
| Default | NONE  |
| Valid values | NONE or AES |

Defines the encryption type for the audit log filter file. The values can be either `NONE`, the default value and no encryption, or `AES`.

### audit_log_filter_file

| Option name | Description |
| --- | --- | 
| Command-line | --audit-log-filter-file |
| Dynamic | Yes  |
| Scope | Global  |
| Data type | Enumeration  |
| Default | ON  |

Defines the name and suffix of the audit log filter file. The plugin writes events to this file. 

The file name and suffix can be a relative path name, the plugin looks for this file in the data directory, or a full path name, the plugin uses the given value. If you use a full path name, be sure that the directory is accessible only to users who need to view the log and the server.

For more information, see [Naming conventions](audit-log-filter-naming.md)

### audit_log_filter_filter_id

| Option name | Description |
| --- | --- | 
| Command-line | --audit-log-filter-file-id |
| Dynamic | Yes  |
| Scope | Global  |
| Data type | Integer  |
| Default | 1  |
| Minimum value | 0 |
| Maximum value | 4292967295 |

Defines the current session's internal ID of the audit log filter file. 

The default value is 0 (zero), which means that the session has no filter assigned.

### audit_log_filter_format

| Option name | Description |
| --- | --- | 
| Command-line | --audit-log-filter-format |
| Dynamic | Yes  |
| Scope | Global  |
| Data type | Enumeration  |
| Default | NEW  |
| Available values | OLD, NEW, JSON |

Defines the audit log filter file format. 

The available values are: [OLD (old-style XML)](audit-log-filter-old.md), [NEW (new-style XML)](audit-log-filter-new.md) and [JSON](audit-log-filter-json.md).

### audit_log_filter_format_unix_timestamp

| Option name | Description |
| --- | --- | 
| Command-line | --audit-log-filter-format-unix-timestamp |
| Dynamic | Yes  |
| Scope | Global  |
| Data type | Boolean  |
| Default | OFF  |

This option is only supported for JSON-format files.

Enabling this option adds a `time` field to JSON-format files. The `time` field indicates the date and time when the event was generated. Changing the value causes a file rotation because all records must either have or do not have the `time` field. This option requires the `AUDIT_ADMIN` privilege along with the `SYSTEM_VARIABLES_ADMIN` privileges. 

This option does nothing when used with other format types.

### audit_log_filter_handler

| Option name | Description |
| --- | --- | 
| Command-line | --audit-log-filter-handler |
| Dynamic | No  |
| Scope | Global  |
| Data type | String  |
| Default | FILE  |

Defines where the plugin writes the audit log filter file. The following values are available:

* `FILE` - plugin writes the log to a location specified in [`audit_log_filter_file`](#audit_log_filter_file)
* `SYSLOG` - plugin writes to the syslog


### audit-log-filter-key-derivation-iterations-count-mean

| Option name | Description |
| --- | --- |
| Command-line | --audit-log-filter-key-derivation-iterations-count-mean |
| Dynamic | Yes  |
| Scope | Global  |
| Data type | Integer  |
| Default | 60000  |
| Minimum value | 1000 |
| Maximum value | 2147483647 |

Defines the mean value of iterations used by the password-based derivation routine used while calculating the encryption key and iv values. The actual iterations count is generated as a random number and deviates no more than 10% from this value.

### audit_log_filter_max_size

| Option name | Description |
| --- | --- |
| Command-line | --audit-log-filter-max-size |
| Dynamic | Yes  |
| Scope | Global  |
| Data type | Integer  |
| Default | 0 |
| Minimum value | 0 |
| Maximum value | 18446744073709551615 |
| Unit | bytes |
| Block size | 4096 |

This option is only supported for JSON-format files.

Defines pruning based on the combined size of the files:

* 0 (zero), which is the default value, disables pruning based on size.
* A value greater than 0 (zero) enables pruning based on size and defines the limit of the combined size. When the files exceed this limit, they can be pruned.

The value is based on 4096 (block size). A value is truncated to the nearest multiple of the block size. If the value is less than 4096, the value is treated as 0 (zero).

If the values for `audit_log_filter_rotate_on_size` and `audit_log_filter_max_size` are greater than 0, we recommend that `audit_log_filter_max_size` value should be at least seven times the `audit_log_filter_rotate_on_size` value. A lower value generates a warning to the error log and may remove all of the rotated logs each time.

Pruning requires the following options:

* `audit_log_filter_max_size`
* `audit_log_filter_rotate_on_size`
* `audit_log_filter_prune_seconds`

### audit_log_filter_password_history_keep_days

| Option name | Description |
| --- | --- |
| Command-line | --audit-log-filter-password-history-keep-days |
| Dynamic | Yes  |
| Scope | Global  |
| Data type | Integer  |
| Default | 0  |

Defines when passwords that are archived and is measured in days.

Encrypted log files have passwords stored in the keyring. The plugin also stores a password history. A password does not expire, despite being past the value, in case the password is used for rotated audit logs.The operation of creating a password also archives the previous password.

The default value is 0 (zero). This value disables the expiration of passwords. Passwords are retained forever. 

If the plugin starts and if encryption is enabled, the plugin checks for an audit log filter encryption password. If a password is not found, the plugin generates a random password.

Call `audit_log_filter_encryption_set() to set a specific password.

### audit_log_filter_prune_seconds

| Option name | Description |
| --- | --- |
| Command-line | --audit-log-filter-prune-seconds |
| Dynamic | Yes  |
| Scope | Global  |
| Data type | Integer  |
| Default | 0  |
| Minimum value | 0 |
| Maximum value | 1844674073709551615 |
| Unit | bytes |

This option is only supported for JSON-format files.

Defines when the audit log filter file is pruned. This pruning is based on the age of the file. The value is measured in seconds. The plugin supports pruning for JSON format only.

A value of 0 (zero) is the default and disables pruning. The maximum value is 18446744073709551615.

A value greater than 0 enables pruning. An audit log filter file can be pruned after this value.

A JSON log pruning operation is based on the values of [`audit_log_filter_rotate_on_size`](#audit_log_filter_rotate_on_size), [`audit_log_filter_max_size`](#audit_log_filter_max_size) and [`audit_log_filter_prune_seconds`](#audit_log_filter_prune_seconds). Setting only one of these values does not cause pruning.

### audit_log_filter_read_buffer_size

| Option name | Description |
| --- | --- |
| Command-line | --audit-log-filter-read-buffer-size |
| Dynamic | Yes  |
| Scope | Global  |
| Data type | Bytes  |
| Default | 32768  |

This option is only supported for JSON-format files.

The size of the buffer for reading from the audit log filter file. The `audit_log_filter_read()` reads only from this buffer size.

| Values | Size |
| --- | --- |
| Default value | 32768 |
| Minimum value | 32768 |
| Maximum value | 4194304 |

### audit_log_filter_rotate_on_size

| Option name | Description |
| --- | --- |
| Command-line | --audit-log-filter-rotate-on-size |
| Dynamic | Yes  |
| Scope | Global  |
| Data type | Integer |
| Default | 0  |

Performs an automatic log file rotation based on the size. The default value is 0 (zero).

If the value is 0 (zero), the plugin does not automatically rotate the log file. If you set the value to less than 4096 resets the value to 0, and does not automatically rotate the log files. You can rotate the log files manually.

If the value is greater than 0, when the log file size exceeds the value, the plugin renames the current file, and opens a new log file using the original name.

If the value is not a multiple of 4096, the plugin truncates the value to the nearest multiple.

### audit_log_filter_strategy

| Option name | Description |
| --- | --- |
| Command-line | --audit-log-filter-strategy |
| Dynamic | No  |
| Scope | Global  |
| Data type | Enumeration  |
| Default | ASYNCHRONOUS  |

Defines the Audit Log filter plugin's logging method. The valid values are the following:

| Values | Description |
| --- | --- |
| ASYNCHRONOUS | Waits until there is outer buffer space |
| PERFORMANCE | If the outer buffer does not have enough space, drops requests |
| SEMISYNCHRONOUS | Operating system permits caching |
| SYNCHRONOUS | Each request calls `sync()` |

### Audit_log_filter_syslog_facility

| Option name | Description |
| --- | --- |
| Command-line | --audit-log-filter-syslog-facility |
| Dynamic | No  |
| Scope | Global  |
| Data type | String  |
| Default | LOG_USER |

Specifies the syslog `facility` value. The option has the same meaning as the appropriate parameter described in the [syslog(3) manual](https://linux.die.net/man/3/syslog).

### Audit_log_filter_syslog_ident

| Option name | Description |
| --- | --- |
| Command-line | --audit-log-filter-syslog-ident |
| Dynamic | No  |
| Scope | Global  |
| Data type | String  |
| Default | percona-audit  |

Defines the `ident` value for the syslog. The option has the same meaning as the appropriate parameter described in the [syslog(3) manual](https://linux.die.net/man/3/syslog).

### Audit_log_filter_syslog_priority

| Option name | Description |
| --- | --- |
| Command-line | --audit-log-filter-syslog-priority |
| Dynamic | No  |
| Scope | Global  |
| Data type | String  |
| Default | LOG_INFO |

Defines the `priority` value for the syslog. The option has the same meaning as the appropriate parameter described in the [syslog(3) manual](https://linux.die.net/man/3/syslog).

## Audit Log Filter status variables

Enabling the Audit Log Filter plugin exposes variables which have information of the plugin's operations. 

### Audit_log_filter_buffer_bypassing_writes



### Audit_log_filter_current_size

### Audit_log_filter_event_max_drop_size

### Audit_log_filter_events

### Audit_log_filter_events_filtered

### Audit_log_filter_events_lost

### Audit_log_filter_events_written





### Audit_log_filter_total_size

### Audit_log_filter_write_waits


## Audit Log Filter functions

### audit_log_filter_encryption_password_get([keyring_id])

### audit_log_filter_flush()

### audit_log_filter_encryption_password_set()

### audit_log_filter_remove_filter(filter_name)

### audit_log_filter_remove_user(username)

### audit_log_rotate()

### audit_log_filter_set_filter(filter_name, definition)

### audit_log_filter_set_user(username, filter_name)

### audit_log_filter_read()

### audit_log_filter_read_bookmark()