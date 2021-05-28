# Gcloud

- store token in gcloud secret manager

```sh
printf 'NZhvjjzJHIesYIKc' | gcloud secrets create \
    pgsql_wp_blog \
    --data-file=- \
    --replication-policy=automatic
```

- creates a new Cloud SQL instance

```sh
gcloud sql instances create pgsql-wp-blog \
    --database-version=POSTGRES_12 \
    --root-password=projects/274373043747/secrets/pgsql_wp_blog \
    --tier=db-f1-micro \
    --region=europe-north1
```
