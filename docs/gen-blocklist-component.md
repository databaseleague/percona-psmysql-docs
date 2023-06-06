# gen_blocklist()

Implemented in Percona Server for MySQL 8.0.33-26.

Replaces one term in a dictionary with a term, selected at random, in another dictionary.

## Parameters

| Parameter | Optional | Description | Type |
| --- | --- | --- | --- |
| term | No | The term to replace | String |
| from_dictionary_name | No | The dictionary that stores the term. | String |
| to_dictionary_name | No | The dictionary that stores the replacement term | String |

## Returns

A term, selected at random, from the dictionary listed in `to_dictionary_name` that replaces the selected term. If the selected `term` is not listed in the `from_dictionary_name` or a dictionary is missing, then the `term` is returned.

## Example

```{.bash data-prompt="mysql>"}
mysql> SELECT gen_blocklist('apple', 'fruit', 'nut');
```

??? example "Expected output"

    ```{.text .no-copy}
    +-----------------------------------------+
    | gen_blocklist('apple', 'fruit', 'nut')  |
    +-----------------------------------------+
    | walnut                                  |
    +-----------------------------------------+
    ```
