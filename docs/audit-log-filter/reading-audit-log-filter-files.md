# Reading Audit Log Filter files

The Audit Log Filter functions can provide a SQL interface to read JSON-format audit log files. The functions cannot read log files in other formats. Configuring the plugin for JSON logging lets the functions use the directory that contains the current audit log filter file and search in that location for readable files. The value of the `audit_log_file_filter` system variable provides the file location, base name, and the suffix and then searches for names that match the pattern.

The file is ignored if a file has been renamed and does not fit the pattern. 

