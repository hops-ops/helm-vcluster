# helm-vcluster

A Crossplane Configuration package that installs the vCluster Helm chart with a minimal, stable interface.

## Overview

`helm-vcluster` renders a single Helm release for vCluster. It exposes only the inputs needed
for chart values, namespace, and release name, keeping the interface stable while allowing full Helm overrides.

vCluster creates virtual Kubernetes clusters that run inside regular namespaces. Each virtual cluster
has its own API server, control plane, and synced resources, providing strong isolation without the
overhead of separate physical clusters.

## Features

- **Minimal Helm interface**: values and overrideAllValues with stable defaults
- **Predictable naming**: defaults to `vcluster-<name>` namespace
- **GitOps friendly**: ships a `.gitops/` deploy chart

## Prerequisites

- Crossplane installed in the cluster
- Crossplane providers:
  - `provider-helm` (>=v1.0.6)
- Crossplane function:
  - `function-auto-ready` (>=v0.6.0)

## Quick Start

```yaml
apiVersion: pkg.crossplane.io/v1
kind: Configuration
metadata:
  name: helm-vcluster
spec:
  package: ghcr.io/hops-ops/helm-vcluster:latest
```

```yaml
apiVersion: helm.hops.ops.com.ai/v1alpha1
kind: VCluster
metadata:
  name: my-vcluster
  namespace: example-env
spec:
  clusterName: example-cluster
  values:
    sync:
      toHost:
        ingresses:
          enabled: true
```

## Development

```bash
make render
make validate
make test
```
