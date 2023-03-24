# Manage the Audit Log Filter files

The Audit Log Filter files have the following potential results:

* Consume a large amount of disk space
* Grow large

You can manage the space by using log file rotation. This operation renames and then rotates the current log file and then uses the original name on a new current log file. You can rotate the file either manually or automatically.

If you use JSON-formatted log files, and if automatic rotation is enabled, you can prune the log file. This pruning operation can be based on either the log file age or combined log file size.

