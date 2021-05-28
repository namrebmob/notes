# Backup, restore, or migrate data volumes

Volumes are useful for backups, restores, and migrations. Use the `--volumes-from` flag to create a new container that mounts that volume.

## Backup a container

For example, create a new container named `dbstore`:

```
docker run -v /dbdata --name dbstore ubuntu /bin/bash
```

Then in the next command, we:

- Launch a new container and mount the volume from the dbstore container
- Mount a local host directory as `/backup`
- Pass a command that tars the contents of the `dbdata` volume to a `backup.tar` file inside our `/backup` directory.

```sh
docker run --rm \
    --volumes-from dbstore \
    -v $(pwd):/backup \
    ubuntu tar cvf /backup/backup.tar /dbdata
```

When the command completes and the container stops, we are left with a backup of our `dbdata` volume.

## Restore container from backup

With the backup just created, you can restore it to the same container, or another that you made elsewhere.

For example, create a new container named `dbstore2`:

```sh
docker run -v /dbdata --name dbstore2 ubuntu /bin/bash
```

Then un-tar the backup file in the new container`s data volume:

```sh
docker run --rm \
    --volumes-from dbstore2 \
    -v $(pwd):/backup \
    ubuntu \
        bash -c "cd /dbdata && tar xvf /backup/backup.tar --strip 1"
```

You can use the techniques above to automate backup, migration and restore testing using your preferred tools.
