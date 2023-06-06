# gen_rnd_email()

Implemented in Percona Server for MySQL 8.0.33-26.

Generates a random email address. The format of the email is `name.surname@domain`. 

## Parameters

| Parameter | Optional | Description | Type |
| --- | --- | --- | --- |
| name_size | Yes | Specifies the number of characters in the name part | Integer |
| surname_size | Yes | Specifies the number of characters in the surname part | Integer |
| domain | Yes | Specifies the number of characters in the domain part | Integer |

The default name for the domain is `example.com`.

## Returns

An email address as a string.

## Example

```{.bash data-prompt="mysql>"}
mysql> SELECT gen_rnd_email();
```

??? example "Expected output"

    ```{.text .no-copy}
    +---------------------------------------+
    | gen_rnd_email()                       |
    +---------------------------------------+
    | sam.smith@example.com                  |
    +---------------------------------------+
    ```
