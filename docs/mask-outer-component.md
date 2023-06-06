# mask_outer()

Implemented in Percona Server for MySQL 8.0.33-26.

Returns the string where a selected outer portion is masked with a substitute character.

## Parameters

| Parameter | Optional | Description | Type |
| --- | --- | --- | --- |
| string | No | The string to be masked | String |
| margin1 | No | The number of characters on the left end of the string to remain unmasked | Integer |
| margin2 | No | The number of characters on the right end of the string to remain unmasked | Integer |
| mask_char | Yes | The masking character | String |

If the `margin1` cannot be a negative number. If the marin1 value is 0, then all of the left end characters are unmasked.

If the `margin2` cannot be a negative number. If the marin1 value is 0, then all of the right end characters are unmasked.

If the sum of `margin1` and `margin2` is greater than the string length, no masking occurs.

If the `mask_char` is not specified, the default is 'X'.

## Returns

A string with the selected characters masked by a specified `mask_char` or that parameter's default value.

## Example

```{.bash data-prompt="mysql>"}
mysql> SELECT mask_outer('123456789', 2, 2); 
```

??? example "Expected output"

    ```{.text .no-copy}
    +------------------------------------+
    | mask_outer('123456789', 2, 2).     |
    +------------------------------------+
    | XX34567XX                          |
    +------------------------------------+
    ```