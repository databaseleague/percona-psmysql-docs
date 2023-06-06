# mask_pan_relaxed()

Implemented in Percona Server for MySQL 8.0.33-26.

Returns a masked payment card Primary Account Number (PAN). The first six numbers and the last four numbers and the rest of the string masked by specified character or `X`.

## Parameters

| Parameter | Optional | Description | Type |
| --- | --- | --- | --- |
| str | No | The string to be masked | String |
| mask_char | Yes | The masking character | String |

The `str` must contain a minimum of 14 or a maximum of 19 alphanumeric characters. 

If the `mask_char` is not specified, the default is 'X'.

## Returns

A string with the first six numbers and the last four numbers and the rest of the string masked by a specified `mask_char` or that parameter's default value (`X`). 

An error occurs if the `str` parameter is not the correct length. Returns a NULL value if the `str` is in an incorrect format or contains a multi-byte character.

## Example

```{.bash data-prompt="mysql>"}
mysql> SELECT mask_pan_relaxed(gen_rnd_pan());
```

??? example "Expected output"

    ```{.text .no-copy}
    +------------------------------------------+
    | mask_pan_relaxed(gen_rnd_pan())          |
    +------------------------------------------+
    | 520754XXXXXX4848                         |
    +------------------------------------------+
    ```
