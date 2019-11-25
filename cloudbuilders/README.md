# Cloud build actions
CI process for init actions scripts.

## Single node cluster
This cloud build `ci_init_actions_single_node_dataproc_cluster.yaml` has next steps
- clone init actions script files to GCS 
- create dataproc cluster and uses these scripts
- delete dataproc cluster 
- remove init actions script files from GCS
 
Run this build to use command:
```
gcloud builds submit \
--config=./cloudbuilders/ci_init_actions_single_node_dataproc_cluster.yaml \
--substitutions=\
_BUILD_BUCKET=${BUILD_BUCKET}/${REPO_NAME}/${BRANCH_NAME}/${SHORT_SHA},\
_CLUSTER_NAME=<CLUSTER_NAME>,\
_REGION=<REGION>,\
_ZONE=<ZONE>
```

## Multi node cluster
```
gcloud builds submit \
--config=./cloudbuilders/ci_init_actions_multi_node_dataproc_cluster.yaml \
--substitutions=\
_BUILD_BUCKET=${BUILD_BUCKET}/${REPO_NAME}/${BRANCH_NAME}/${SHORT_SHA},\
_CLUSTER_NAME=<CLUSTER_NAME>,\
_REGION=<REGION>,\
_ZONE=<ZONE>
```