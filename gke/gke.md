

### Custom Service Account for Gke Node.
```bash

export PROJECT_ID=your-project-id

gcloud iam service-accounts create base-gke-node-sa \
  --display-name=base-gke-node-sa

gcloud projects add-iam-policy-binding $PROJECT_ID \
  --member "serviceAccount:base-gke-node-sa@$PROJECT_ID.iam.gserviceaccount.com" \
  --role roles/logging.logWriter

gcloud projects add-iam-policy-binding $PROJECT_ID \
  --member "serviceAccount:base-gke-node-sa@$PROJECT_ID.iam.gserviceaccount.com" \
  --role roles/monitoring.metricWriter

gcloud projects add-iam-policy-binding $PROJECT_ID \
  --member "serviceAccount:base-gke-node-sa@$PROJECT_ID.iam.gserviceaccount.com" \
  --role roles/monitoring.viewer

gcloud projects add-iam-policy-binding $PROJECT_ID \
  --member "serviceAccount:base-gke-node-sa@$PROJECT_ID.iam.gserviceaccount.com" \
  --role roles/stackdriver.resourceMetadata.writer

gcloud projects add-iam-policy-binding $PROJECT_ID \
  --member "serviceAccount:base-gke-node-sa@$PROJECT_ID.iam.gserviceaccount.com" \
  --role roles/artifactregistry.reader

```

### If your cluster already exists, you can now create a new node pool with this new service account:
```bash

export NODE_POOL_NAME=the-base-worker-pools

export CLUSTER_NAME=your-gke-cluster-name

gcloud container node-pools create $NODE_POOL_NAME \
  --service-account=base-gke-node-sa@$PROJECT_ID.iam.gserviceaccount.com \
  --cluster=$CLUSTER_NAME

```
