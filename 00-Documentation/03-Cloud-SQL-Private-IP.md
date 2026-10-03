# Create Cloud SQL Private IP

The following `gcloud` commands are for creating a temporal `Cloud SQL` MySQL instance with Private IP to practice. 

## Create Infrastructure

### Enable Service Networking API

```shell
gcloud services enable servicenetworking.googleapis.com
```

### Create Allocated IP Range for Services

```shell
gcloud compute addresses create google-managed-services-default \
  --global \
  --purpose VPC_PEERING \
  --prefix-length 16 \
  --network default \
  --description "google-managed-services-default"
```

### Create Private Connection to Services

```shell
gcloud services vpc-peerings connect \
  --service servicenetworking.googleapis.com \
  --ranges google-managed-services-default \
  --network default
```

### Create Cloud SQL MySQL Instance

```shell
gcloud sql instances create ums-db-private-instance \
  --database-version MYSQL_9_7 \
  --edition enterprise \
  --tier db-custom-1-3840 \
  --zone us-central1-a \
  --availability-type zonal \
  --storage-type HDD \
  --storage-size 10GB \
  --storage-auto-increase \
  --network default \
  --no-assign-ip \
  --no-backup \
  --no-deletion-protection \
  --root-password KalyanReddy13
```

## Delete Infrastructure

### Delete Cloud SQL MySQL Instance

```shell
gcloud sql instances delete ums-db-private-instance
```

### Delete Private Connection to Services

This command failed with:

```plaintext
ERROR: (gcloud.services.vpc-peerings.delete) The operation "operations/dcf.p40-1020163930984-2d98d27b-4366-4f81-a245-ebf2b0fc9871" resulted in a failure "Failed to delete connection; Producer services (e.g. CloudSQL, Cloud Memstore, etc.) are still using this connection.
```

```shell
gcloud services vpc-peerings delete \
  --service servicenetworking.googleapis.com \
  --network default
```

### Delete Allocated IP Range for Services

```shell
gcloud compute addresses delete google-managed-services-default \
  --global
```
