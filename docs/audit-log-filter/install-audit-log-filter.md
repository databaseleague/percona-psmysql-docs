# Install the Audit Log Filter

The `plugin_dir` system variable defines the plugin library location. If needed, at server startup, set the `plugin_dir` variable.

When upgrading a MySQL installation, plugins are not automatically upgraded. You may need to manually load certain functions or reinstall the plugin after the MySQL upgrade.

In the `share` directory, locate the `audit_log_filter_linux_install.sql `script.

```{.bash data-prompt="$"}
$ mysql -u root -p < audit_log_filter_linux_install.sql
```

To verify the plugin installation, run the following command:

```{.bash data-prompt="mysql>"}
mysql> SELECT PLUGIN_NAME, PLUGIN_STATUS FROM INFORMATION_SCHEMA.PLUGINS WHERE PLUGIN_NAME LIKE `audit%';
```

??? example "Expected output"

    ```text
    +-------------+---------------+
    | PLUGIN_NAME | PLUGIN_STATUS |
    +-------------+---------------+
    | audit_log   | ACTIVE        |
    +-------------+---------------+
    ```



