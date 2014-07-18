Quick Query
===========

NO HAVE CODE NOW
----------------

PHP query tool with sort sintax.

"A very fast way to make a query, only for lazy programmers" - Marcel Lopes

Usage:
------
This example is a quick overview:

```php
<?php
  require 'dir_of_qq/quickquery.basic.php';
  $data = qq('table_name#?', [1]);
  // query: "SELECT field1, field2, field3 FROM table_name WHERE id = ? LIMIT 1;" params: [1]
  print_r($data);
  /* output:
  array(
    'table_name' => array(
      'id' => 1,
      'field1' => 'value1',
      'field2' => 'value2',
      'field3' => 3,
      ...
    )
  )
  */
```
This example is a join:

```php
<?php
  $data = qq('table_name,table_two,table_last#1');
  /* query: "SELECT
    table_name.field1 as table_name__field1,
    table_name.field2 as table_name__field2,
    table_name.field3 as table_name__field3,
    table_two.field1 as table_two__field1,
    table_two.field2 as table_two__field2,
    table_two.field3 as table_two__field3,
    table_last.field1 as table_last__field1,
    table_last.field2 as table_last__field2,
    table_last.field3 as table_last__field3
    FROM table_name
    LEFT JOIN table_two ON table_name.field1 = table_two.field2
    LEFT JOIN table_last ON table_name.field2 = table_last.field1
    WHERE table_name.id = 1
    LIMIT 1;"
  */
  print_r($data);
  /* output:
  array(
    'table_name' => array(
      'id' => 1,
      'field1' => 'value1',
      'field2' => 'value2',
      'field3' => 3,
      ...
    ),
    'table_two' => array(
      'field1' => 'value1',
      'field2' => 'value2',
      'field3' => 3,
      ...
    ),
    'table_last' => array(
      'field1' => 'value1',
      'field2' => 'value2',
      'field3' => 3,
      ...
    )
  )
  */
?>
```

Sintax
------
The basic sintax is:

`table_name{@field1,field2,field3,...,_count}[field1=1],table2{field1,field2,field3,...}[field1=1|field2=$qq]`
The `#` is for retrive a unique element from primary key of first table.
The fields in `{...}` is the fields in `SELECT (...)`
The fields in `[...]` is the fields in `WHERE (...)`
If you use `@` before the first field, you say to use `GROUP BY (...)` is used fields in `SELECT (...)`, minus the agrupators consts

## Agrupators:

 * `_count` like a SQL `COUNT(field)`, you can use `_count:field4`, if is not present is used `*`
 * `_sum` like a SQL `SUM`, you can use `_sum:field4`
 * `_avg` like a SQL `AVG`, you can use `_avg:field4`
 * The complex fields you need to store the params in Model


# Params:

You can pass params to send to PDO, instead to use in string:

```php
<?php
  require 'dir_of_qq/quickquery.basic.php';
  $data = qq('table_name#?', [1]);
  // query: "SELECT field1, field2, field3 FROM table_name WHERE id = ? LIMIT 1;", params: [1]
```
If you pass params as array:

```php
<?php
  require 'dir_of_qq/quickquery.basic.php';
  $data = qq('table_name[id=?]', [ [1, 2, 3] ]);
  // query: "SELECT field1, field2, field3 FROM table_name WHERE id IN (?, ?, ?);", params: [1, 2, 3]
```

## Types of comparation in WHERE:
  
  * `=`: is used `=` or `IN`
  * `!=`: is used `!=` or `NOT IN`
  * `<>`: is used `!=` or `NOT IN`
  * `>=`: is used `>=`
  * `<=`: is used `<=`
  * `>`: is used `>`
  * `<`: is used `<`
  * `:=`: is used `BETWEEN`, you need to send 2 params


```php
<?php
  require 'dir_of_qq/quickquery.basic.php';
  $data = qq('table_name[id=?]', [ [1, 2, 3] ]);
  // query: "SELECT field1, field2, field3 FROM table_name WHERE id IN (?) LIMIT 1;", params: [ [1, 2, 3] ]
```
  

## Sub queries:
You can pass a subquery to send to PDO, instead to use then in string:

```php
<?php
  require 'dir_of_qq/quickquery.basic.php';
  $data = qq('table_name[field3=$qq&field2=?]', ['qq' => 'table_two{field1}[field2=?]', 'qq_params' => [1], 2]);
  /* query: "SELECT
    table_name.field1 as table_name__field1, table_name.field2 as table_name__field2, table_name.field3 as table_name__field3
    FROM table_name
    WHERE table_name.field3 IN (SELECT table_two.field1 FROM table_two WHERE table_two.field2 = ?) AND table_name.field2 = ?",
    params: [1, 2]
  */
```

# Sort

Todo

# Connect tool

Todo

# Models

Todo

# Migrations

Todo
