# Audit Log Filter format - JSON

The JSON format uses JSON arrays that contain properties as a key-value pair. These arrays represent an event in the audit. Some pairs are listed in every audit record. The audit record type determines if other key-value pairs are listed. The order of the pairs within an audit record is not guaranteed. The value description may be truncated.

Certain statistics, such as query time and size, are only available in the JSON format and help detect activity outliers when analyzed. 

```json

[
  {
    "timestamp": "2023-03-29 11:17:03",
    "id": 0,
    "class": "audit",
    "server_id": 1
  },
  {
    "timestamp": "2023-03-29 11:17:05",
    "id": 5,
    "class": "command",
    "event": "command_start",
    "connection_id": 1,
    "command_data": {
      "name": "command_start",
      "status": 0,
      "command": "query"}
  },
  {
    "timestamp": "2023-03-29 11:17:05",
    "id": 6,
    "class": "general",
    "event": "log",
    "connection_id": 11,
    "account": { "user": "root[root] @ localhost []", "host": "localhost" },
    "login": { "user": "root[root] @ localhost []", "os": "", "ip": "", "proxy": "" },
    "general_data": {
      "command": "Query",
      "sql_command": "create_table",
      "query": "CREATE TABLE t1 (c1 INT)",
      "status": 0}
  },
  {
    "timestamp": "2023-03-29 11:17:05",
    "id": 7,
    "class": "query",
    "event": "query_start",
    "connection_id": 11,
    "query_data": {
      "query": "CREATE TABLE t1 (c1 INT)",
      "status": 0,
      "sql_command": "create_table"}
  },
  {
    "timestamp": "2023-03-29 11:17:05",
    "id": 8,
    "class": "query",
    "event": "query_status_end",
    "connection_id": 11,
    "query_data": {
      "query": "CREATE TABLE t1 (c1 INT)",
      "status": 0,
      "sql_command": "create_table"}
  },
  {
    "timestamp": "2023-03-29 11:17:05",
    "id": 10,
    "class": "general",
    "event": "status",
    "connection_id": 11,
    "account": { "user": "root[root] @ localhost []", "host": "localhost" },
    "login": { "user": "root[root] @ localhost []", "os": "", "ip": "", "proxy": "" },
    "general_data": {
      "command": "Query",
      "sql_command": "create_table",
      "query": "CREATE TABLE t1 (c1 INT)",
      "status": 0}
  },
  {
    "timestamp": "2023-03-29 11:17:05",
    "id": 11,
    "class": "command",
    "event": "command_end",
    "connection_id": 1,
    "command_data": {
      "name": "command_end",
      "status": 0,
      "command": "query"}
  }
]
```

