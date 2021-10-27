```bash

gcloud projects get-iam-policy [PROJECT_ID] --flatten="bindings[].members" --format='table(bindings.role)' --filter="bindings.members:[SERVICE_ACCOUNT_NAME]"
ROLE: roles/iam.workloadIdentityUser

ROLE: roles/pubsub.publisher

ROLE: roles/storage.objectViewer

```
