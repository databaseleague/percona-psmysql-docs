# gen_dictionary()

Implemented in Percona Server for MySQL 8.0.33-26.

Returns a term from a dictionary selected at random.

## Parameters

| Parameter | Optional | Description | Type |
| --- | --- | --- | --- |
| dictionary_name | No | Select the random term from this dictionary | String |


## Returns

A random term from the dictionary listed in `dictionary_name`.

## Example

```{.bash data-prompt="mysql>"}
mysql> SELECT gen_dictionary('trees');
```

??? example "Expected output"

    ```{.text .no-copy}
    +--------------------------------------------------+
    | gen_dictionary('trees')                          |
    +--------------------------------------------------+
    | Norway spruce                                    |
    +--------------------------------------------------+
    ```