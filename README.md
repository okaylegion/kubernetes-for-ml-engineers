## Kubernetes for ML Engineers

### What is this repo about?



### Steps

## Create local Kubernetes cluster with `kind`

We will use `kind` to create a local Kubernetes cluster. It will be a simple cluster that
will run entirely on your machine, using as Kubernetes nodes simple Docker containers.

Production cluster creation, configuration, and mainteincance is something you won't be doing in your
day to day as ML Engineer. This is something MLOps Engineers and DevOps Engineers do.

A local cluster like the one we are creating here is useful for development and CI pipelines,
where you need a minimal cluster to run integration tests for your applications.


Back to work. Install `kind` in your machine:

- On MacOS:
    ```bash
    brew install kind
    ```

- On Windows with Chocolatey:
    ```bash
    choco install kind
    ```

- On Linux:
    ```bash
    # For AMD64 / x86_64
    [ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.27.0/kind-linux-amd64
    # For ARM64
    [ $(uname -m) = aarch64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.27.0/kind-linux-arm64
    chmod +x ./kind
    sudo mv ./kind /usr/local/bin/kind
    ```

Let's define a custom cluster configuration with 2 worker nodes.

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
    kubeadmConfigPatches:
      - |
        kind: InitConfiguration
        nodeRegistration:
          kubeletExtraArgs:
            node-labels: "CriticalAddonsOnly=true,eks-k8s-version=1.29"

  - role: worker
    kubeadmConfigPatches:
      - |
        kind: JoinConfiguration
        nodeRegistration:
          kubeletExtraArgs:
            node-labels: "CriticalAddonsOnly=true,eks-k8s-version=1.29"

  - role: worker
    labels:
      "CriticalAddonsOnly": "true"
      "eks-k8s-version": "1.29"
```

Create the cluster:

```bash
kind create cluster --config kind.yaml --name local-kind-cluster
```

Set the kubectl context to the local cluster, so you can interact with the cluster using `kubectl`:

```bash
kubectl config use-context kind-local-kind-cluster
```

Get the list of nodes in the cluster:

```bash
kubectl get nodes
```
```
NAME                               STATUS   ROLES           AGE   VERSION
local-kind-cluster-control-plane   Ready    control-plane   23m   v1.32.2
local-kind-cluster-worker          Ready    <none>          23m   v1.32.2
local-kind-cluster-worker2         Ready    <none>          23m   v1.32.2
```

Voila! You have a local Kubernetes cluster running on your machine.

Let's now move on to the ML engineering work.

## Write the `deployment.yaml` and `service.yaml` files













