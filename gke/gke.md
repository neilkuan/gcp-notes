

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




### Autopilot cluster use `[your-project-id]-compute@developer.gserviceaccount.com` (gce default service account to grant permission).
> I meet this issue, xxxxxxxxxx-compute@developer.gserviceaccount.com role binding at `Editor`, `Cloud Pub/Sub Service Agent` and `Service Account Admin`, but I saw permission deny at autopilot cluster pod logs `"E0103 05:24:48.668010       1 server.go:178] Failed to process request with tag kube_kube-system_gke-metrics-agent-hbt9s_gke-metrics-agent_stderr: rpc error: code = PermissionDenied desc = The caller does not have permission"`
<img width="1130" alt="截圖 2022-01-03 下午1 37 33" src="https://user-images.githubusercontent.com/46012524/147902124-397ac46d-3561-472c-aa43-8acffa66d1e8.png">

1. use `gcloud` command sdk to show this service account binding role. 
```bash
gcloud projects get-iam-policy [your-project-id] --flatten="bindings[].members" --format="table(bindings.role)" --filter="bindings.members:[your-project-id]-compute@developer.gserviceaccount.com"
                         # <- I got null!!!.
```


2. create role binding with this service account.
```bash
gcloud projects add-iam-policy-binding [your-project-id] --member='serviceAccount:[your-project-id]-compute@developer.gserviceaccount.com' --role='roles/editor'

Updated IAM policy for project [your-project-id].
bindings:
...
- members:
  - serviceAccount:[your-project-id]-compute@developer.gserviceaccount.com
  role: roles/editor
...
etag: XXXXXXXX0123=
version: 1
```

3. check role binding again
```bash
gcloud projects get-iam-policy [your-project-id] --flatten="bindings[].members" --format="table(bindings.role)" --filter="bindings.members:developer.gserviceaccount.com"
ROLE: roles/editor
```

