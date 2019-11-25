# Cloud build actions

- `ci_init_actions.yaml` tests init actions on dataproc single node cluster 
Run this build to use command:
```
gcloud builds submit \
--substitutions=_BUILD_BUCKET=gcp-cicd-artifacts\
--config=./cloudbuilds/ci_init_actions.yaml
```