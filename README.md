# Kustomize Workshop

This repo is for practicing the basics of Kustomize.

## Prerequisites
- Kubernetes cluster
- `kubectl` client

## Kustomize
Kustomize lets you customize raw, template-free YAML files for multiple purposes, leaving the original YAML untouched and usable as is.
- Kustomize is a patching framework
- Enables environment specific changes to be introduced without duplicating YAML unnecessarily
- Unlike templating frameworks, all YAML can be directly applied
- Kustomize is included in kubectl and oc starting in 1.14

<br/>
Kustomize is organized in a hierarchical directory structure of *bases* and *overlays*.
- A *base* is a directory with a kustomization.yaml file that contains a set of resources and associated customization.
  - A base has no knowledge of an overlay and can be used in multiple overlays.
- An *overlay* is a directory with a kustomization.yaml file that references other kustomization directories
  - An overlay may aggregate multiple bases/overlays and may apply additional customization to them.

## Running the demo
### Base
First, let's take a look at the base directory:
```
base
├── index-html-configmap.yaml
├── kustomization.yaml
├── nginx-service.yaml
└── nginx.yaml
```

It includes nginx service and it's configuration.

### Overlay
We have 2 overlays to simulate different environments: dev and prod:
```
overlay
├── dev
│   └── kustomization.yaml
└── prod
    └── kustomization.yaml
```

In both overlays we are patching the config map and the node port, while we are also patching the container image.

### Build the Kustomization
We can build the Kustomization manifests with `kubectl` client
```bash
kubectl kustomize overlay/dev
```

### Apply the manifests
```bash
kubectl apply -k overlay/dev
kubectl apply -k overlay/prod
```

Now we can acess the service in node ports 32001-32002 and see the differences.
