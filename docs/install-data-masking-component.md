# Install the data masking component

Percona Server for MySQL 8.0.33-26 adds the data masking component. Percona Server for MySQL has the [Data Masking plugin](./data-masking-plugin.md). 

Before installing the component, you must [uninstall the data masking plugin and all of the functions](./data-masking-plugin.html#uninstalling-the-plugin). The removal avoids conflicts.

The component has the following parts:

* A `mysql` system table used to store the terms and dictionaries
* A `component_masking` component that exposes the masking functions as a service interface
* A `component_masking_functions` component which contains the loadable functions

The `MASKING_DICTIONARIES_ADMIN` privilege may be required by some functions.

## Install the component

The following steps install the component:

1. Create `masking_dictionaries`.

```{.bash data-prompt="mysql>"}
mysql> CREATE TABLE IF NOT EXISTS
mysql.masking_dictionaries(
    Dictionary VARCHAR(256) NOT NULL,
    Term VARCHAR(256) NOT NULL,
    UNIQUE INDEX dictionary_term_idx (Dictionary, Term),
    INDEX dictionary_idx (Dictionary)
) ENGINE = InnoDB DEFAULT CHARSET=utf8mb4;
```

2. Install the data masking components and the loadable functions.

```{.bash data-prompt="mysql>"}
mysql> INSTALL COMPONENT 'file://component_masking';
mysql> INSTALL COMPONENT 'file://component_masking_functions';
```

## Uninstall the component

The following steps uninstall the component:

1. Uninstall the component with `UNINSTALL_COMPONENT` and the loadable functions.

```{.bash data-prompt="mysql>"}
mysql> UNINSTALL COMPONENT 'file://component_masking';
mysql> UNINSTALL COMPONENT 'file://component_masking_functions';
```

2. Drop `masking_dictionaries`.

```{.bash data-prompt="mysql>"}
mysql> DROP TABLE mysql.masking_dictionaries;
```
