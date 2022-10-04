# Using the Keyring Plugin

*Percona Server for MySQL* may use either of the following plugins:

* keyring_file stores the keyring data locally

* keyring_vault provides an interface for the database with a HashiCorp Vault
server to store key and secure encryption keys.

!!! note

    The `keyring_file` plugin should not be used for regulatory compliance.

To install the plugin, follow the [installing and uninstalling plugins](https://dev.mysql.com/doc/refman/8.0/en/plugin-loading.html) instructions.

## Loading the Keyring Plugin

You should load the plugin at server startup with the `-early-plugin-load`
option to enable keyrings.

!!! warning 

    Only one keyring plugin should be enabled at a time. Enabling multiple keyring plugins is not supported and may result in data loss.

We recommend the plugin should be loaded in the configuration file to facilitate
recovery for encrypted tables. Also, the redo log and the undo log encryption cannot
be used without `--early-plugin-load`. The normal plugin load happens too late
in startup.

!!! note

    The keyring_vault extension, “.so” and the file location for the vault configuration should be changed to match your operating system’s extension and the file location in your operating system.

To use the keyring_vault, you can add this option to your configuration file:

```text
[mysqld]
early-plugin-load="keyring_vault=keyring_vault.so"
loose-keyring_vault_config="/home/mysql/keyring_vault.conf"

The keyring_vault extension, ".so" and the file location for the vault
configuration should be changed to match your operating system's extension
and operating system location.
```

You could also run the following command which loads the keyring_file plugin:

```shell
$ mysqld --early-plugin-load="keyring_file=keyring_file.so"
```

!!! note

    If a server starts with different plugins loaded early, the `--early-plugin-load` option should contain the plugin names in a double-quoted list with each plugin name separated by a semicolon. The use of double quotes ensures the semicolons do not create issues when the list is executed in a script.

Apart from installing the plugin you also must set the
keyring_vault_config variable to point to the keyring_vault
configuration file.

The keyring_vault_config file has the following information:

* `vault_url` - the Vault server address

* `secret_mount_point` - the mount point name where the keyring_vault stores the keys.

* `secret_mount_point_version` - the `KV Secrets Engine version (kv or kv-v2)` used. Implemented in *Percona Server for MySQL* 8.0.23-14.

* `token` - a token generated by the Vault server

* `vault_ca [optional]` - if the machine does not trust the Vault’s CA certificate, this variable points to the CA certificate used to sign the Vault’s certificates

This is an example of a configuration file:

```text
vault_url = https://vault.public.com:8202
secret_mount_point = secret
secret_mount_point_version = AUTO
token = 58a20c08-8001-fd5f-5192-7498a48eaf20
vault_ca = /data/keyring_vault_confs/vault_ca.crt
```

!!! warning

    Each `secret_mount_point` must be used by only one server. If multiple servers use the same secret_mount_point, the behavior is unpredictable.

The first time a key is fetched from a keyring, the keyring_vault
communicates with the Vault server to retrieve the key type and data.

## secret_mount_point_version information

Implemented in *Percona Server for MySQL* 8.0.23-14, the `secret_mount_point_version`
can be either a `1`, `2`, `AUTO`, or the `secret_mount_point_version`
parameter is not listed in the configuration file.

| Value            | Description                                          |       
|----------------- | ---------------------------------------------------- | 
| 1                | Works with `KV Secrets Engine - Version 1 (kv)`. When forming key operation URLs, the `secret_mount_point` is always used without any transformations. For example, to return a key named `skey`, the URL is <vault_url>/v1/<secret_mount_point>/skey  |
| 2                | Works with `KV Secrets Engine - Version 2 (kv)` The initialization logic splits the `secret_mount_point` parameter into two parts:<ul><li>The `mount_point_path` - the mount path under which the Vault Server secret was created</li><li>The `directory_path` - a virtual directory suffix that can be used to create virtual namespaces with the same real mount point</li></ul> For example, both the `mount_point_path` and the `directory_path` are needed to form key access URLs: <vault_url>/v1/<mount_point_path/data/<directory_path>/skey |
| AUTO | An autodetection mechanism probes and determines if the secrets engine version is `kv` or `kv-v2` and based on the outcome will either use the `secret_mount_point` as is, or split the `secret_mount_point` into two parts.|
| Not listed| If the `secret_mount_point_version` is not listed in the configuration file, the behavior is the same as `AUTO`.|

If you set the `secret_mount_point_version` to `2` but the path pointed
by `secret_mount_point` is based on `KV Secrets Engine - Version 1 (kv)`,
an error is reported and the plugin fails to initialize.

If you set the `secret_mount_point_version` to `1` but the path pointed
by `secret_mount_point` is based on `KV Secrets Engine -
Version 2 (kv-v2)`, the plugin initialization succeeds but any MySQL
keyring-related operations fail.

## Upgrading from 8.0.22-13 or earlier to 8.0.23-14 or later

The `keyring_vault` plugin configuration files created before
*Percona Server for MySQL* 8.0.23-14 work only with `KV Secrets Engine -
Version 1 (kv)` and do not have the `secret_mount_point_version`
parameter. After the upgrade to 8.0.23-14 or later, the
`secret_mount_point_version` is implicitly considered `AUTO` and the
information is probed and the secrets engine version is determined to `1`.

## Upgrading from Vault Secrets Engine Version 1 to Version 2

You can upgrade from the Vault Secrets Engine Version 1 to Version 2.
Use either of the following methods:

* Set the `secret_mount_point_version` to `AUTO` or the variable is not set in the `keyring_vault` plugin configuration files in all Percona Servers. The `AUTO` value ensures the autodetection mechanism is invoked during the plugin initialization.

* Set the `secret_mount_point_version` to `2` to ensure that plugins do not initialize unless the `kv` to `kv-v2` upgrade completes.

!!! note

    The `keyring_vault` plugin that works with `kv-v2` secret engines does not use the built-in key versioning capabilities. The keyring key versions are encoded into key names.

## KV Secret Engine considerations for upgrading from 5.7 to 8.0

When you upgrade from *Percona Server for MySQL* 5.7.32 or older, you can only use
`KV Secrets Engine 1 (kv)`. You can upgrade to any version of
*Percona Server for MySQL* 8.0. Both the old `keyring_vault` plugin and new
`keyring_vault` plugin work correctly with the existing Vault Server
data under the existing `keyring_vault` plugin configuration file.

If you upgrade from *Percona Server for MySQL* 5.7.33 or newer, you have the following options:

* If you are using `KV Secrets Engine 1 (kv)` you can upgrade with any version of *Percona Server for MySQL* 8.0.

* If you are using `KV Secrets Engine 2 (kv-v2)` you can upgrade with *Percona Server for MySQL* 8.0.23 or newer. *Percona Server for MySQL* 8.0.23.14 is the first version of the 8.0 series which has the `keyring_vault` plugin that supports `kv-v2`.

A user-created key deletion is only possible with the use of the keyring_udf
plugin and deletes the key from the in-memory hash map and the Vault server.
You cannot delete system keys, such as the master key.

This plugin supports the SQL interface for keyring key management described in
[General-Purpose Keyring Key-Management Functions](https://dev.mysql.com/doc/refman/8.0/en/keyring-udfs-general-purpose.html)
manual.

The plugin library contains keyring user-defined functions which allow
access to the internal keyring service functions. To enable the functions, you
must enable the `keyring_udf` plugin:

```sql
mysql> INSTALL PLUGIN keyring_udf SONAME 'keyring_udf.so';
```

!!! note

    The `keyring_udf` plugin must be installed. Using the user-defined functions without the `keyring_udf` plugin generates an error.

You must also create keyring encryption user-defined functions.

## System Variables

### `keyring_vault_config`

| Option       | Description            |
|--------------|------------------------|
| Command-line | --keyring-vault-config |
| Scope        | Global                 |
| Dynamic      | Yes                    |
| Data type    | Text                   |
| Default      |

This variable is used to define the location of the keyring_vault_plugin
configuration file.

### `keyring_vault_timeout`

| Option       | Description             |
|--------------|-------------------------|
| Command-line | --keyring-vault-timeout |
| Scope        | Global                  |
| Dynamic      | Yes                     |
| Data type    | Numeric                 |
| Default      | 15                      |

Set the duration in seconds for the Vault server connection timeout. The
default value is `15`. The allowed range is from `0` to `86400`. The
timeout can be also disabled to wait an infinite amount of time by setting
this variable to `0`.