# gen_rnd-Canada-sin()

Implemented in Percona Server for MySQL 8.0.33-26.

Returns a number from a defined range.

## Parameters

| Parameter | Optional | Description | Type |
| --- | --- | --- | --- |
| lower | No | The lower boundary of the range | Integer |
| upper | No | The upper boundary of the range | Integer |

The `upper` parameter must be an integer higher than the `lower` parameter.

## Returns

An integer, selected at random, from a range defined by the `lower` parameter and the `upper` parameter.

## Example

```{.bash data-prompt="mysql>"}
mysql> SELECT gen_range(10, 100);
```

??? example "Expected output"

    ```{.text .no-copy}
    +--------------------------------------+
    | gen_range(10,100)                    |
    +--------------------------------------+
    | 56                                   |
    +--------------------------------------+
    ```