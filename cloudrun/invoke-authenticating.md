[Authenticating developers ](https://cloud.google.com/run/docs/authenticating/developers#gcloud)

```bash
curl -H "Authorization: Bearer $(gcloud auth print-identity-token)" https://xxxx-xxxx-uc.a.run.app
```
