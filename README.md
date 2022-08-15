# Mirror MinIO clusters with MinIO client

This simple Kubernetes Deployment sets up a Pod with a MinIO client which can
mirror files between two existing MinIO clusters.

## Setup

The configuration is based on Secrets for the root users and passwords of the
cluster.

1. In the Namespace of the source MinIO cluster [create a Secret][1] called
`minio-dest-admin` containing the `root-user` and the `root-password` of the
destination MinIO cluster.
1. If you do not already have a Secret containing the root credentials of the
source MinIO cluster, please [create one][1] called `minio-source-admin`
containing the keys `root-user` and `root-password` with their corresponding
values. Otherwise make sure to configure the name of the existing Secret in
the [`minio-client`][3] Deployment.
1. Define the source and destination clusters in the
[`minio-clusters`][2] ConfigMap.
1. Apply the ConfigMap [`minio-clusters`][2] to the Namespace of the source
MinIO cluster.
1. Before you apply the Deployment please consider this:
You can run the `mc mirror` command in line 30 of the [`minio-client`][3]
Deployment with `--fake` instead of `--watch` to ensure the setup works. See
the Pods logs for its output after applying the file to the cluster. If you
want to go full force, just change the flag to `--watch`.
1. Apply the deployment to the Namespace of the source MinIO cluster to start
mirroring.
1. Delete the deployment to stop mirroring.

[1]: https://kubernetes.io/docs/concepts/configuration/secret/#use-case-pods-with-prod-test-credentials
[2]: minio-clusters.cm.yaml
[3]: minio-client.deployment.yaml
