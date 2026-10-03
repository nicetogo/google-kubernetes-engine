# Create Cloud SQL Public IP

The following `gcloud` commands are for creating a temporal `Cloud SQL` MySQL instance with Public IP to practice. 

## Create Infrastructure

### Create Cloud SQL MySQL Instance

```shell
gcloud sql instances create ums-db-public-instance \
  --database-version MYSQL_9_7 \
  --edition enterprise \
  --tier db-custom-1-3840 \
  --zone us-central1-a \
  --availability-type zonal \
  --storage-type HDD \
  --storage-size 10GB \
  --storage-auto-increase \
  --assign-ip \
  --authorized-networks "0.0.0.0/0" \
  --no-backup \
  --no-deletion-protection \
  --root-password KalyanReddy13
```

## Delete Infrastructure

### Delete Cloud SQL MySQL Instance

```shell
gcloud sql instances delete ums-db-public-instance
```
