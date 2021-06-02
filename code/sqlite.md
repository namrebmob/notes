# sqlite

## Export

```sh
sqlite3 ./db.sqlite3
.output ./dump.sql
.dump
.exit
```

## Import

```sh
sqlite3
sqlite> .read dump.sql
# or
cat dump.sql | sqlite3 db.sqlite3
```
