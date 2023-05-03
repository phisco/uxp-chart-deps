## POC of UXP Helm chart using dependency

# Structure

```
.
└── charts
    └── universal-crossplane
        ├── Chart.yaml
        ├── charts
        │   └── crossplane -> the exact copy of the crossplane chart
        ├── templates -> any additional component we might want to specify
        │   ├── NOTES.txt
        │   ├── _helpers.tpl
        │   └── bootstrapper
        └── values.yaml -> defining only additional values and overriding the needed ones for crossplane
```

`charts/universal-crossplane/charts/crossplane` is an exact copy of the upstream [Helm chart](https://github.com/crossplane/crossplane/tree/master/cluster/charts/crossplane), vendored just to ease future modifications if needed.

# Usage

## Installation

```bash
$ kind create cluster
$ helm upgrade --install uxp charts/universal-crossplane
NAME: uxp
LAST DEPLOYED: Wed May  3 16:32:57 2023
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
By proceeding, you are accepting to comply with terms and conditions in https://licenses.upbound.io/upbound-software-license.html

✨ Thank you for installing Universal Crossplane!
🚀 Install packages from https://marketplace.upbound.io to get started!
```

## Uninstall

```bash
$ helm uninstall uxp
release "uxp" uninstalled
```

## Upgrade from upstream

```bash
$ helm repo add crossplane-stable https://charts.crossplane.io/stable
$ helm repo update
$ helm upgrade --install crossplane crossplane-stable/crossplane
Release "crossplane" does not exist. Installing it now.
NAME: crossplane
LAST DEPLOYED: Wed May  3 16:38:20 2023
NAMESPACE: default
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
Release: crossplane

Chart Name: crossplane
Chart Description: Crossplane is an open source Kubernetes add-on that enables platform teams to assemble infrastructure from multiple vendors, and expose higher level self-service APIs for application teams to consume.
Chart Version: 1.12.1
Chart Application Version: 1.12.1

Kube Version: v1.26.3

$ helm upgrade --install crossplane cluster/charts/universal-crossplane
Release "crossplane" has been upgraded. Happy Helming!
NAME: crossplane
LAST DEPLOYED: Wed May  3 16:39:03 2023
NAMESPACE: default
STATUS: deployed
REVISION: 2
TEST SUITE: None
NOTES:
By proceeding, you are accepting to comply with terms and conditions in https://licenses.upbound.io/upbound-software-license.html

✨ Thank you for installing Universal Crossplane!
```
All should be working fine and the image used should be now the one from the uxp chart.

