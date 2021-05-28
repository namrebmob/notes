# sqlite

## Export
```sh
sqlite3 ./db.sqlite3
.output ./backup.sqlite3
.dump
.exit
```

## Import
```sh
sqlite> .read db.sql
# or
cat db.sql | sqlite3 database.db
```
