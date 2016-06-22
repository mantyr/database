# Modified Golang database/sql

[![Build Status](https://travis-ci.org/mantyr/database.svg?branch=master)](https://travis-ci.org/mantyr/database) [![GoDoc](https://godoc.org/github.com/mantyr/database?status.png)](http://godoc.org/github.com/mantyr/database)

## Installation

    $ go get github.com/mantyr/database
    $ go get github.com/mantyr/go-sql-driver-mysql

## Changelog

*    **2015-06-05** : Add Easier get NULL values. [Oleg Shevelev][mantyr].
*    **2015-05-18** : Add Easier access to the data. [Oleg Shevelev][mantyr].

## Examples

```Go
package main

import (
    "github.com/mantyr/database/sql"
  _ "github.com/mantyr/go-sql-driver-mysql"
    "fmt"
    "strconv"
)

var db *sql.DB

func db_get_example(id int) (int, string, int) {
    if id == 0 {
        return 0, "", 0
    }
    query := `
        SELECT
            "10"   as item_id,
            "name" as item_name,
            "180"  as item_pid
        FROM table_name
        WHERE id = "`+strconv.Itoa(id)+`"
        LIMIT 1
    `
    res := db.QueryRow(query)
    row, err := res.ScanRow()
    if err == nil {
        return row.GetInt("item_id"), row.Get("item_name"), row.GetInt("item_pid")
    }
    return 0, "", 0
}

func get_db() (*sql.DB, error) {
    db, err := sql.Open("mysql", "username:password@tcp(localhost:3306)/db?charset=utf8")
    return db, err
}


func main() {
    db, _ = get_db()
    item_id, item_name, item_pid := db_get_example(10)

    fmt.Println(item_id, item_name, item_pid)
}
```

[mantyr]: https://github.com/mantyr
