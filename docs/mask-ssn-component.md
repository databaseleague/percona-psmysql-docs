# mask_ssn()

Implemented in Percona Server for MySQL 8.0.33-26.

Returns a masked United States Social Security Number(SSN). The mask replaces the SSN number with the specified character except for the last four digits.

## Parameters

| Parameter | Optional | Description | Type |
| --- | --- | --- | --- |
| str | No | The string to be masked | String |
| mask_char | Yes | The masking character | String |

The `str` accepts either of the following:

* Nine integers, no separator symbol
* Nine integers in the `***-**-****` pattern. The `-` is a separator character.

If the `mask_char` is not specified, the default is `*`.

## Returns

A string with the selected characters masked by a specified `mask_char` or that parameter's default value. Returns a NULL value if the `str` is in an incorrect format or contains a multi-byte character.

## Example

```{.bash data-prompt="mysql>"}
mysql> SELECT mask_ssn('555-55-5555');
```

??? example "Expected output"

    ```{.text .no-copy}
    +-------------------------+
    | mask_ssn('555-55-5555') |
    +-------------------------+
    | ***-**-5555             |
    +-------------------------+
    ```