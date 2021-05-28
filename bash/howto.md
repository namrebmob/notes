# Bash how to's

## Append text to a file that you don’t have write permissions to:

```bash
echo "this is a new line" | sudo tee -a file.txt
```

## Recursively change the files and directories permissions:

```bash
find /var/www/html -type f -exec chmod u=rw,go=r {} \;
find /var/www/html -type d -exec chmod u=rwx,go=rx {} \;
```

When using find with -exec, the chmod command is run for each found entry. Use the xargs command to speed up the operation by passing multiple entries at once:

```bash
find /var/www/html -type f -print0 | xargs -0 chmod 644
find /var/www/html -type d -print0 | xargs -0 chmod 755
```

## Count files:

```bash
# count files in current directory:
find <PATH> -maxdepth 1 -type f | wc -l

# recursively count files:
find <PATH> -type f | wc -l
```

