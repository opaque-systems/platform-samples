# Quickstart

This sample shows you how to deploy a simple Nginx Web server on the Opaque
platform.

## Deploy it

**First**, change the namespace in which the sample is to be deployed to the
namespace in which OPAQUE is deployed in your Kubernetes cluster.

For example, if your deployment of OPAQUE lives in a namespace called
`opaque-platform`, change all instances of `namespace: default` in
`nginx-deployment.yaml` accordingly.

**Second**, adjust the `imagePullSecrets` field in `nginx-deployment.yaml` to
point to the Secret that contains the container registry credentials where you
are hosting OPAQUE's container images. These images are provided to you during
the onboarding process.

**Third**, ascertain which Platform Class ID corresponds to your deployment of
OPAQUE. To do so, run:

```sh
opaque oas manifests
```

* For GCP, you want the Class ID for the Platform layer manifest for TEE type
  TDX that is not revoked;
* For Azure, you want the Class ID for the Platform layer manifest for TEE type
  SEV-SNP that is not revoked.

In `nginx-owd.yaml`, set the `platform-class-id` field to this Class ID.

**Finally**, register and deploy the workload. Navigate to the quickstart
directory and run:

```sh
opaque auth login  # Run only once, if not done already.

opaque workload register -f nginx-owd.yaml | kubectl apply -f -
```

Inspect the state of the workload:

```sh
opaque workload status K8S_NAMESPACE DEPLOYMENT_NAME
```

Ensure that you replace `K8S_NAMESPACE` with the namespace that you identified
in the first step (where OPAQUE is deployed), and `DEPLOYMENT_NAME` with the
name of the Deployment resource in `nginx-deployment.yaml`.

## What it deploys

The sample creates:

1. A ConfigMap with some sample data for Nginx to serve;
2. A Deployment that runs Nginx using a public image;
3. A LoadBalancer Service to allow traffic into Nginx.
