# Create GKE Cluster

The following `gcloud` commands are for creating a temporal `GKE` cluster to practice. 

## Create Infrastructure

### Capture the Project ID

```shell
PROJECT_ID=$(gcloud config get-value project)
```

### Create Cloud Router

```shell
gcloud compute routers create gke-router \
  --region us-central1 \
  --network default
```

### Create Cloud NAT

```shell
gcloud compute routers nats create gke-nat \
  --router gke-router \
  --region us-central1 \
  --auto-allocate-nat-external-ips \
  --nat-all-subnet-ip-ranges
```

### Create External IP Address

This is necessary for some labs. Create it only if you are going to work on the labs.

```shell
gcloud compute addresses create gke-ingress-extip1 --global

gcloud compute addresses describe gke-ingress-extip1 --global

gcloud compute addresses list
```

### Create Private Regional GKE Cluster

```shell
gcloud container clusters create gke-dev \
  --region us-central1 \
  --node-locations us-central1-a,us-central1-b,us-central1-c \
  --num-nodes 1 \
  --enable-autoscaling \
  --min-nodes 1 \
  --max-nodes 3 \
  --machine-type e2-medium \
  --spot \
  --disk-size 30GB \
  --disk-type pd-standard \
  --enable-private-nodes \
  --enable-ip-alias \
  --master-ipv4-cidr 172.16.0.0/28 \
  --enable-master-authorized-networks \
  --master-authorized-networks "0.0.0.0/0" \
  --workload-pool="${PROJECT_ID}.svc.id.goog" \
  --addons GcpFilestoreCsiDriver,HttpLoadBalancing \
  --enable-secret-manager
```

## Delete Infrastructure

### Delete GKE Cluster

```shell
gcloud container clusters delete gke-dev --region us-central1
```

### Delete Cloud NAT

```shell
gcloud compute routers nats delete gke-nat \
  --router gke-router \
  --region us-central1 
```

### Delete Cloud Router

```shell
gcloud compute routers delete gke-router \
  --region us-central1
```

### Delete the External IP Address

```shell
gcloud compute addresses delete gke-ingress-extip1 --global
```
